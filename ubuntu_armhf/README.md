Build and Run ARM Docker Images on x86 Host
=================================================

- Install QEMU  
1.apt-get  
    `apt-get update && apt-get install -y --no-install-recommends qemu-user-static binfmt-support`
    `update-binfmts --enable qemu-arm`  
    `update-binfmts --display qemu-arm`  
2.source (make sure that the qemu-arm-static binary is under /usr/bin/)  
    `git clone git://git.qemu.org/qemu.git`  
    `cd qemu`  
    `./configure --target-list=arm-linux-user --static`  
    `make`  


<del>- Register qemu-arm with binfmt module (#issue:Permission denied)</del>  
<del>`mount binfmt_misc -t binfmt_misc /proc/sys/fs/binfmt_misc`</del>  
<del>`echo ":arm:M::\x7fELF\x01\x01\x01\x00\x00\x00\x00\x00\x00\x00\x00\x00\x02\x00\x28\x00:\xff\xff\xff\xff\xff\xff\xff\x00\xff\xff\xff\xff\xff\xff\xff\xff\xfe\xff\xff\xff:/usr/bin/qemu-arm-static:" > /proc/sys/fs/binfmt_misc/register`</del>  


- Copy qemu-arm-static to DOCKERFILE_DIR  
    `cp /usr/bin/qemu-arm-static DOCKERFILE_DIR`

- Add "COPY" instruction below the "FROM" instruction in Dockerfile  
    `COPY qemu-arm-static /usr/bin/qemu-arm-static`  

- Build Docker Image with the Modified Dockerfile  
    `docker build -t IMAGE_NAME<:Tag> .`

- Run Docker Image on x86 Host  
    `docker run -it IMAGE_NAME<:Tag>`
