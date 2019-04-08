**Save image local:**
1) _docker save -o 'path for generated tar file' 'image name'_
Then copy your image to a new system with regular file transfer tools such as cp or scp. After that you will have to load the image into Docker:

2) _docker load -i 'path to image tar file'_
PS: You may need to sudo all commands.

EDIT: You should add filename (not just directory) with -o, for example:
docker save -o c:/myfile.tar centos:16

