# docker
docker file

# mac docker 磁盘清理
  * 删除无用images
      * `docker rmi $(docker images -q -f dangling=true)`
  * 转换Docker.qcow2文件

```
Connect to the VM with screen ~/Library/Containers/com.docker.docker/Data/com.docker.driver.amd64-linux/tty and then login as root
Fill up the disk with zeroes:
dd if=/dev/zero of=/var/tempfile
This'll take at least couple of minutes on SSD. I wouldn't bother if you're on spinning rust...
Then you need to rm /var/tempfile, logout of the VM, and quit the Docker client entirely. Now we can recompress the disk:

$ pwd
/Users/nick/Library/Containers/com.docker.docker/Data/com.docker.driver.amd64-linux
$ mv Docker.qcow2 Docker.qcow2.original
$ du -hs Docker.qcow2.original
12G     Docker.qcow2.original
$ qemu-img convert -O qcow2 Docker.qcow2.original Docker.qcow2
$ rm Docker.qcow2.original
$ du -hs Docker.qcow2
772M    Docker.qcow2
```

