#docker #docker-image
Docker Images are named `Dockerfile`.

## This Are option used in an Dockerfile:
| Dockerfile Instruction | Explanation |
| --- | --- |
| FROM | To specify the base image which can be pulled from a container registry (Docker hub, GCR, Quay, ECR, etc.) |
| RUN | Executes commands during the image build process. |
| ENV | Sets environment variables inside the image. It will be available during build time and in a running container. If you want to set only build-time variables, use ARG instruction. |
| COPY | Copies local files and directories to the image. |
| EXPOSE | Specifies the port to be exposed for the Docker container. |
| ADD | A more feature-rich version of the COPY instruction. Allows copying from a URL, source and tar file auto-extraction into the image. However, the COPY command is recommended over ADD. Use curl or get with RUN to download remote files. |
| WORKDIR | Sets the current working directory. Reusable in the Dockerfile to set a different working directory. Instructions like RUN, CMD, ADD, COPY, and ENTRYPOINT get executed in that directory. |
| VOLUME | Used to create or mount a volume to the Docker container. |
| USER | Sets the user name and UID when running the container. Use to set a non-root user of the container. |
| LABEL | Used to specify metadata information of the Docker image. |
| ARG | Used to set build-time variables with key and value. ARG variables are not available when the container is running. Use ENV to persist a variable on a running container. |
| CMD | Used to execute a command in a running container. Only one CMD is used, the last one applies. Can be overridden from the Docker CLI. |
| ENTRYPOINT | Specifies the commands that will execute when the Docker container starts. Defaults to /bin/sh -c if not specified. Can be overridden with the --entrypoint flag using the CLI. Refer to CMD vs ENTRYPOINT for more information. |
Example:
```Dockerfile
LABEL maintainer="contact@devopscube.com"
FROM ubuntu:18.04
RUN apt-get -y update && apt-get -y install nginx
COPY files/default /etc/nginx/sites-available/default
COPY files/index.html /usr/share/nginx/html/index.html
EXPOSE 80
CMD ["/usr/sbin/nginx", "-g", "daemon off;"]
```

## <font color="#ffff00">Docker Ignore file</font> 
The "<font color="#e36c09">.dockerignore</font>" <u>file is used in a Docker build context to specify a list of files and directories that should not be included in the build context</u>, i.e., they will be ignored by the Docker build process. The file is usually placed in the root directory of the build context and contains one file/directory path per line. The ".dockerignore" file helps reduce the size of the build context, as well as reduces build time, by excluding files that are not necessary for building the Docker image.
## Dockerfile Best Practices
Some of the `Dockerfile` practices which we should follow:
1.  Use a `.dockerignore` file to exclude unnecessary files and directories to increase the build’s performance.
2.  Use **trusted base images** only and keep updating the images periodically.
3.  Each instruction in the `Dockerfile` adds an extra layer to the Docker image. Minimize the number of layers by consolidating the instructions to increase the build’s performance and time.
4.  Run as a **Non-Root User** to avoid security breaches.
5.  Reduce the image size for faster deployment and avoid installing unnecessary tools in your image. Use minimal images to reduce the attack surface.
6.  Use specific tags over the latest tag for the image to avoid breaking changes over time.
7.  Avoid using multiple `RUN` commands as it creates multiple cacheable layers which will affect the efficiency of the build process.
8.  Never share or copy the application credentials or any sensitive information in the `Dockerfile`. If you use it, add it to .`dockerignore`
9.  Use `EXPOSE` and `ENV` commands as late as possible in `Dockerfile`.