# Build `amd64` and `arm64` Docker images and push them to Docker Hub

---

**The purpose of this repository is to share how to generate and publish Docker images to Docker Hub for various architectures.**

Since Apple's New silicon chip doesn't share the default Intel chips architecture, it has an impact on how Docker is run and is used in the new Mac M1 and M2 machines.

To distribute Docker images that can generate containers on both architectures, I have compiled on this repository two methods for building either Docker images from a Dockerfile that use 'buildx' (1, 2).

- Method one: GitHub Action (3)

- Method two: Docker commands (4). 

---

## Method one: GitHub Actions
*Adapted from (3)*

The GitHub Action workflow build and pushes direct to DockerHub the two Docker images. 
There are some requirements:
- The base image should exist in both architectures `arm64` and `amd64`.
- There are two SECRETS to add to the GitHub repository: `DOCKER_USERNAME`, `DOCKER_PASSWORD`.

## Method two: Docker commands
*Adapted from (4)*  

You should have Docker Desktop installed.

``` 
docker buildx create --name=mybuilder --use  
docker buildx inspect --boostrap # extra
docker ps  # extra
docker buildx build -t username/image --platform linux/amd64,linux/arm64 --push . # If you are running the command in the folder where the Dockerfile is
```
*Note*  
In case of getting the following error run `docker login --username=<username>`  
```
------
 > exporting to image:
------
error: failed to solve: server message: insufficient_scope: authorization failed
```


## References
1 - [Building Multi-Arch Images for Arm and x86 with Docker Desktop (2019)](https://www.docker.com/blog/multi-arch-images/) - Adam Parco
2 - [Multi-Platform Docker Builds (2020)](https://www.docker.com/blog/multi-platform-docker-builds/) - Adrian Mouat  
3 - [How to Rapidly Build Multi-Architecture Images with Buildx (2022)](https://www.docker.com/blog/how-to-rapidly-build-multi-architecture-images-with-buildx/) - Tyler Charboneau  
4 - [Building multi-architecture images with GitHub Actions (2020)](https://blog.oddbit.com/post/2020-09-25-building-multi-architecture-im/) - Lars Kellogg-Stedman    
5 - [DockerCon 2021: I Have an M1 Mac, Now What? Docker in a Multi-arch World](https://www.youtube.com/watch?v=pvaQcMrvMJo) - Tonis Tiigi  
6 - [Multi-platform images - Docker documentation](https://docs.docker.com/build/building/multi-platform/)

