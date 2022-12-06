# Create `amd64` and `arm64` versions of the same Dockerfile and push them to Docker Hub

---

**The goal of this repository is to share how to create and push Docker images to Docker Hub for different architectures.**
The new apple silicon chip is used in the new Mac M1 and M2 computers affecting the way Docker is run, since it doesn't share the architecture of Intel chips.
In order to circulate Docker images that can generate containers on both architectures, I compilate on this repository two methods available to build from a Dockerfile both Docker images using the command `buildx`.
- Method one: Continuous integration with GitHub Actions 
- Method two: Docker commands needed to create the Docker images manually from the command line

---

## Method one: GitHub Actions
*Adapted from (1)*

The GitHub Action workflow build and pushes direct to DockerHub the two Docker images. 
There are some requirements:
- The base image should exist in both architectures `arm64` and `amd64`.
- There are three SECRETS to add to the GitHub repository: `DOCKER_USERNAME`, `DOCKER_PASSWORD` and `DOCKER_IMAGE`.

## Method two: Docker commands
*Adapted from 2*  
  
1 - `docker buildx create --name=mybuilder --use`  
2 (extra) - `docker buildx inspect --boostrap`  
3 (extra) - `docker ps`  
4 - `docker buildx build -t username/image --platform linux/amd64,linux/arm64 --push .`  
  
*Note*
In case of getting the following error run `docker login --username=<username>`  
```
------
 > exporting to image:
------
error: failed to solve: server message: insufficient_scope: authorization failed
```


## References
1 - [Building multi-architecture images with GitHub Actions](https://blog.oddbit.com/post/2020-09-25-building-multi-architecture-im/) - Fri, Sep 25, 2020 by Lars Kellogg-Stedman  
2 - [DockerCon 2021: I Have an M1 Mac, Now What? Docker in a Multi-arch World](https://www.youtube.com/watch?v=pvaQcMrvMJo) - Tonis Tiigi

