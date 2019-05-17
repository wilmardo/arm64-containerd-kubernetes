## Compile runc for arm on amd64

```
docker run --rm --privileged multiarch/qemu-user-static:register --reset
docker run -v $(pwd)/files:/data -e GOPATH=/tmp -it --rm multiarch/ubuntu-debootstrap:arm64-bionic /bin/bash
apt-get update
apt-get install -y golang git libseccomp-dev
go get github.com/opencontainers/runc
cd $GOPATH/src/github.com/opencontainers/runc
make
mv /tmp/src/github.com/opencontainers/runc/runc /data/runc
```