# shared-userspace module

[**Package DB** (owner)](https://admin.fedoraproject.org/pkgdb/package/modules/shared-userspace/) |
[**F26 modulemd** (source)](http://pkgs.fedoraproject.org/cgit/modules/shared-userspace.git/tree/shared-userspace.yaml?h=f26) |
[**PDC** (result)](https://pdc.fedoraproject.org/rest_api/v1/unreleasedvariants/?active=True&variant_name=shared-userspace)


This modules contains some "components" that cannot be physically separated
from each other or the distro at this point. This module provides common build-
and runtime dependencies that are shared between the other modules.  As this is
work in progress, other modules that depend on this module might not work
anymore in the next iteration.

common-build-dependencies and shared-userspace modules have some overlap as
some packages that are required for builds also have a runtime component. Other
packages are required only for a couple of other packages where it might be
easier to just ship them together in a different module. These choices makes
deciding where to put a certain package a little difficult.

Examples to explain how to decide weather a package belongs into c-b-d, s-u or none of them:


libtool

It is used by many packages during builds, but it also has libtool-ltdl as a library that many packages get linked with. That's the main reason for me to put libtool into the shared-userspace module.



autoconf

It is used by many packages during a build, no part of it is needed afterwards. It needs to be in common-build-dependencies.



python-pymongo

It is a runtime requirement by only two packages:

#> dnf repoquery -q --releasever 26  --whatrequires python-pymongo
mongodb-test-0:3.4.3-1.fc26.x86_64
python2-oslo-cache-tests-0:1.14.0-3.fc26.noarch

It is required by a couple of packages as a build requirement:

#> dnf repoquery -q --enablerepo fedora-source --releasever 26 --arch src --whatrequires python-pymongo

mongodb-0:3.4.3-1.fc26.src
python-kombu-1:4.0.2-4.fc26.src
python-mongoengine-0:0.11.0-2.fc26.src
python-oslo-cache-0:1.14.0-3.fc26.src
python-pysaml2-0:3.0.2-6.fc26.src

This belongs into the mongodb module, but that decision might be revisited when
more packages depend on python-pymongo




## Current state

| State | Description |
|-------|-------------|
| ✓ YES | **Build passes** in the infra (all build deps are ok) |
| ✗ NO | **Installs** on the Base Runtime (all runtime deps are ok) |
| ✓ NO | **No bootstrap** - uses only proper modules |
| ✗ NO | General **tests are in dist-git** |
| ✗ NO | General **tests pass** |
| ? | Meets the **Fedora Module Packaging Guidelines** |
<!--
| ✓ YES | yes! |
| ✗ NO  | no! |
-->

### Notes

