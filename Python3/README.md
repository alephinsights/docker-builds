## Docker image for ML and Bayes

## TL;DR

1. **Check**: nvidia driver, docker and [nvidia-docker](https://github.com/NVIDIA/nvidia-docker) installed?
1. **Build**: `docker build -t petecog/python3 -f Dockerfile.gpu .`
2. **Run**: 	`docker run -u 1000:1000 -it -p 8888:8888 -p 6006:6006 -v /<HOST DIR YOU WANT TO USE ON GUEST>:/project petecog/python3 bash`


## Specs
This is what you get out of the box when you create a container with the provided image/Dockerfile:
* Base docker image - Ubuntu 16.04
* Python 3.5 - and various standard bits and pieces (numpy, pandas etc)
