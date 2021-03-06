## Docker image for ML and Bayes

## TL;DR

1. **Check**: nvidia driver, docker and [nvidia-docker](https://github.com/NVIDIA/nvidia-docker) installed?
1. **Build**: `docker build -t petecog/cuda-pymc3:gpu -f Dockerfile.gpu .`
2. **Run**: 	`docker run --runtime=nvidia -u 1000:1000 -it -p 8888:8888 -p 6006:6006 -v /<HOST DIR YOU WANT TO USE ON GUEST>:/project petecog/cuda-pymc3:gpu bash`

## Credits and Refs
- Built with reference to:
	- [https://github.com/floydhub/dl-docker](https://github.com/floydhub/dl-docker)
	- [https://github.com/senbon/dockerfiles](https://github.com/senbon/dockerfiles)


## Specs
This is what you get out of the box when you create a container with the provided image/Dockerfile:
* Base docker image - [aleksaro/python3-nn](https://hub.docker.com/r/aleksaro/python3-nn/)
* PyMC3 and it's varous dependencies- [https://github.com/pymc-devs/pymc3](https://github.com/pymc-devs/pymc3)
* Theano
* scikit-learn
* Python 3.5 - and various standard bits and pieces (numpy, pandas etc)


## Setup
### Prerequisites
1. Install Docker following the installation guide for your platform: [https://docs.docker.com/engine/installation/](https://docs.docker.com/engine/installation/)

2. This requires NIVIDIA GPU setup to be working with nvidia-docker: [https://github.com/NVIDIA/nvidia-docker](https://github.com/NVIDIA/nvidia-docker), follow the instructions [here](https://github.com/NVIDIA/nvidia-docker/wiki/Installation). This will install a wrapper for the docker CLI. It takes care of setting up the Nvidia host driver environment inside the Docker containers and a few other things.

### Build the Docker image

```bash
git clone https://github.com/alephinsights/docker-builds
cd CUDA-PyMC3-TensorFlow
docker build -t petecog/cuda-pymc3:gpu -f Dockerfile.gpu .
```

This will build a Docker image named `cuda-pymc3` with the tag `gpu`.

## Running the Docker image as a Container
Once built you can run the container.

```
docker --runtime=nvidia run -u 1000:1000 -it -p 8888:8888 -p 6006:6006 -v /<HOST DIR YOU WANT TO USE ON GUEST>:/project petecog/cuda-pymc3:gpu bash
```

## Running the notebook

Once up, the image should launch jupyter, which will be accessible via the address in the terminal. If not, or you need to re launch, use the script here:

`/home/user/run_jupyter.sh`

Then point your browser at the url it suggests, and you will be good to go!

## Check that it's using CUDA/NVIDIA card as you expect.

In the container terminal:

`nvidia-smi`

you should get something back that looks like this, showing your driver version and system load on the host machine.

```
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 381.09                 Driver Version: 381.09                    |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  GeForce GTX 1080    Off  | 0000:01:00.0      On |                  N/A |
| 16%   57C    P0    38W / 180W |   2019MiB /  8110MiB |      0%      Default |
+-------------------------------+----------------------+----------------------+

+-----------------------------------------------------------------------------+
| Processes:                                                       GPU Memory |
|  GPU       PID  Type  Process name                               Usage      |
|=============================================================================|
+-----------------------------------------------------------------------------+
```

Alternativly, from either an ipython shell or note book run `import pymc3`. You should see something the following, which indicates it's found your GPU:


```
Using gpu device 0: GeForce GTX 1080 (CNMeM is enabled with initial size: 10.0% of memory, cuDNN 5110)

```

## Things to do / get around to

 - build a leaner build
 - publish image to docker hub
 - solve the writing data to host as root problem [a lead on a solution](https://denibertovic.com/posts/handling-permissions-with-docker-volumes/) at the moment the user is 'hard coded' in the docker build file to be 1000:1000, which is fine if your host user matches, but this is not portable
 	- ...possibly... move to a start up script - something like [this](https://stackoverflow.com/questions/34610800/how-to-create-a-docker-image-container-with-same-file-rights-as-host-user)
	 	- creation of user (specify user in run command)
	 	- copy of run_jupyter.sh to new user DIR
	 	- chown of /home/user
