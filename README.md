# Container Hardening

IronBank is a container Hardening solution that implements the [DevSecOps Enterprise Container Hardening Guide](https://dl.dod.cyber.mil/wp-content/uploads/devsecops/pdf/Final_DevSecOps_Enterprise_Container_Hardening_Guide_1.2.pdf).


The overall requirements:
* Use an IronBank Base Image
* Provide list of all external dependencies needed to build image
* Scan image using multiple scanning tools
  * OpenSCAP/InSpec
  * Prisma/StackRox
  * Anchore
* Provide justifications for any un-mitigated vulnerabilities
* Build in an egress-limited environment


This Zarf package solves all the requirements of the hardened image

* The IronBank image is downloaded and provided as part of the zarf package
* All dependencies are provided as part of the Zarf package
* Scans can be done on the SBOMs provided as part of the Zarf Package:

```bash
$ zarf package inspect zarf-package-blah.tar.zst --sbom-out sbom
$ cat ./sbom/*.json | grype
```

* Justifications are provided in the VEX document, and pulled down with the IronBank image [#475](https://github.com/defenseunicorns/zarf/issues/475)
* Builds are done wherever the consumer wants them to be done, so the location of that build process is controlled by the consumer


## Running this Poc

```bash
$ zarf package create
$  docker images | grep "fakeironbank" # fails
```

```bash
$ zarf package deploy
$  docker images | grep "fakeironbank" # find the image!
$ cat base-vulnerabilities.json # vulns in image.
$ cat vulnerabilities.json  # vulns in SBOMs in zarf package
$ docker ls 