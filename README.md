# SFTP-FTP
docker file to build sftp &amp; ftp based on centos7
#From the root of the project directory where the Dockerfile currently is, you can now build the image:
docker build -t <IMAGE_TAG> 
# tart a new container in the background based on the image we just built: 
docker run -d --restart=always --name <CONTAINER_NAME> -p 2222:22 -v <SFTP_DIRECTORY_ROOT>:<SFTP_DIRECTORY_ROOT> <IMAGE_TAG>
# 2222 is the port we're using locally — You might want to avoid using 22 since that one is probably in use for your regular SSH server if you have one.
# Provide the right volume mounts using the -v option (you can mount more than one location). Ideally you'd want to mount some sort of root directory in which you'll later create the user directories for SFTP users.
# You can now start and stop the new container manually:
docker start <CONTAINER_NAME>
docker stop <CONTAINER_NAME>
# Adding user accounts
# The new container doesn't do anything without local accounts.
# push your own /etc/passwd and /etc/shadow files to the container or even the image itself but it's easier and somewhat safer to do it manually from a local shell on the container.
# Once you're ready to create a new account and the container is already running in the backround, you can get a shell to it using docker exec:
docker exec -it <CONTAINER_NAME> /bin/sh
# Then add the user (and group):
addgroup --gid <GID> <GROUP_NAME>
adduser -h <CHROOT_DIRECTORY> -H -u <UID> -G <GROUP_NAME> <USER_NAME>
# We have to use two seperate commands and these specific arguments because Alpine is using busybox to 
provide these commands and these are different than user addgroup or adduser you'd find on a fresh Ubuntu.
