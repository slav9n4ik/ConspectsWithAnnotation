**Save image local:**
1) _docker save -o 'path for generated tar file' 'image name'_
Then copy your image to a new system with regular file transfer tools such as cp or scp. After that you will have to load the image into Docker:

2) _docker load -i 'path to image tar file'_
PS: You may need to sudo all commands.

EDIT: You should add filename (not just directory) with -o, for example:
docker save -o c:/myfile.tar centos:16

**Main commands:**

_docker build -t 'tag name' 'path to Dockerfile'_ - build image by Docker file

_docker run --name 'container name' -v 'volume path in pc:path in container' -p 'port in:port out' -d 'iamge name'_ - run docker container

_docker stop 'name or id of container'_ - stop container

_docker rm 'name or id of container'_ - remove container

_docker rmi 'name or id of image'_ - remove image _(-f - force flag)_
