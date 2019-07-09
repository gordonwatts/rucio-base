# rucio-base

Tools to build a docker container that contains `rucio` and the certificates needed to run it in an modern LHC environment, and not much more! 

## Introduction

Using the GRID properly can be painful, especially with the required security infrastructure. This docker image attempts to put it all in one place. While this image can be used stand-alone, it is more likely useful as a building block for other images.

## Usage

In order to use rucio you'll need a GRID certificate. Assuming that your `usercert.pem` and `userkey.pem` files are located in `<dir>`, then the following commands check that all is right with `rucio` and your account.

Start the container with:
```
docker run --rm -v <dir>:/root/certs  -it gordonwatts/rucio-base:latest
```

The successful completion of the following commands will assure that rucio and your account are correctly
configured:

```
cd .globus/
cp /root/certs/*.pem .
chmod 400 /root/.globus/userkey.pem
chmod 444 /root/.globus/usercert.pem
export RUCIO_ACCOUNT=<rucio-username>
voms-proxy-init -voms atlas
rucio ping
```

A few quick points:

- Why not mount the user certificate directly into the `.globus` directory? That works fine
  on Linux if you've applied the proper permissions. It is not posible on Windows. Modify to suit
  your uses. 

## Building

As a package of certificates must be downloaded from CERN
(or some other end point), this can't be build up on docker
hub. For the instructions below you'll need an account on
`lxplus.cern.ch`.

1. From the root directory of this repo, run the script `./scripts/sync_cert_with_ATLAS.sh <lxplus-username>`.
   - If you are on windows, do this from a WSL bash shell (e.g. `Ubuntu`).
   - Depending on your connection, this could take a minute or two. there are 1000's of small files that have to be copied.
2. Determine the version of `rucio` you want to build against. Look for the version numbers on [Docker Hub](https://hub.docker.com/r/rucio/rucio-clients/tags). For example, `release-1.19.8`.
   - I suggest finding a machine that you know is setup properly, and looking at what version of `rucio` they are running with `rucio --version`.
3. Use `docker` to build the image: `docker build --rm -f "Docker\Dockerfile" --build-arg RUCIO_VERSION=release-1.19.8  -t gordonwatts/rucio-base:v1.0.0-alpha.1 .`