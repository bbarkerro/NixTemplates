# NixTemplates
Several basic templates that are meant to be used as the basis for other Singularity Spec files
using the [Nix](https://nixos.org/nix/) package manager.

## Background

### Nix

Nix provides reproducible builds for software, all the way down to the system level.
This is done by way of keeping track of which commit revision of
[nixpkgs](https://github.com/nixos/nixpkgs) was used at the time of the build.
THe user can also pin versions of particular software dependencies by
coding them into the nix expression (think build script).

### Singularity

Singularity is like a Docker container, but without process isolation.
So it isn't a process container, but it is a filesystem container.
Unlike Docker, Singularity provides a non-layered filesystem. This may
have benefits for reproducibility, but also means increased file size if
a user is building multiple image files based on other singularity images.

### Nix and Singularity

Nix wouldn't work well with layering anyway: one benefit of nix is the nix-store,
which is a local store on the system which puts all builds of software that
are hashed based on the nix expression being used to build the software, and any
inputs to that nix expression (so you can have multiple alternative builds of the
same software). A single Singularity image, that holds a custom nix expression,
should be ideal to build an individual image for a particular use case, or even
multiple use cases: multiple use cases can be packaged in a single Singularity
image and separated by using different nix expressions: they all share the same
nix store, so when there are common dependencies, no file system redundancy occurs.


In short, Nix provides build-level customization and reproducibility, which is important
for future work on the project to proceed smooth, whereas Singularity provides
an archive of the existing build state, that is important for both immediate usage,
and as a last resort for users who can't get the build procedure to work down the
road for some unforseen reason.

# Building Images

## Docker

### nix_ubuntu_base

```bash
cd Docker
source build.sh

```

### nix_ubuntu_openmpi

```bash
source Docker/OpenMPI/build.sh
```