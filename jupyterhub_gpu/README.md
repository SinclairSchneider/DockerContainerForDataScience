# Jupyterhub + CUDA Docker image

The following Dockerfiles build a Jupyterhub Server Environment that offers
a Jupyterlap server instance running on port 8000.
In this folder you will find the two subfolders "jupyterhub_gpu" and "jupyterhub_gpu_local".
Each of these folders has its own Dockerfile in it.
The Dockerfile inside the "jupyterhub_gpu" folder has the full
building plan for a Jupyterhub-Server environment along with some extras. 
The Dockerfile inside the folder "jupyterhub_gpu_local" only contains the
finalization steps such as setting a password.

The image comes with the following properties:
- Inheritance from image nvidia/cuda:11.4.1-cudnn8-devel-ubuntu20.04
- curl
- python3-pip
- jupyterhub (python 3)
- notebook (python 3)
- tensorflow-gpu (python 3)
- torch==1.9.0+cu111 (python 3)
- torchvision==0.10.0+cu111 (python 3)
- torchaudio==0.9.0 (python 3)
- wget (python 3)
- pandas (python 3)
- transformers (python 3)
- numpy (python 3)
- sklearn (python 3)
- matplotlib (python 3)
- nltk (python 3)
- spacy (python 3)
- seaborn (python 3)
- unzip
- npm
- nodejs
- sudo

## To just run the container
To just run the container, clone this git repository and got to the directory "jupyterhub_gpu/jupyterhub_gpu_local/" by the following command:
```commandline
git clone https://github.com/SinclairSchneider/DockerContainerForDataScience
cd DockerContainerForDataScience/jupyterhub_gpu/jupyterhub_gpu_local/
```
At the first run the image needs to be initialized. Do this with the following command.
```commandline
docker image build .
```
The response then might look as follows
```commandline
Sending build context to Docker daemon  2.048kB
Step 1/3 : FROM sinclair1992/jupyterhub_gpu
latest: Pulling from sinclair1992/jupyterhub_gpu
xxxxxxxxxxxx: Already exists 
xxxxxxxxxxxx: Already exists 
xxxxxxxxxxxx: Already exists 
xxxxxxxxxxxx: Already exists 
xxxxxxxxxxxx: Already exists 
xxxxxxxxxxxx: Already exists 
xxxxxxxxxxxx: Already exists 
xxxxxxxxxxxx: Already exists 
xxxxxxxxxxxx: Already exists 
xxxxxxxxxxxx: Already exists 
xxxxxxxxxxxx: Pull complete 
Digest: sha256:b3fda4eaf37348ca1e5cfd1a59c953a65e161076b0f0fadb3678639f7dc21fa9
Status: Downloaded newer image for sinclair1992/jupyterhub_gpu:latest
 ---> xxxxxxxxxxxx
Step 2/3 : RUN useradd -mg users -G sudo -s /bin/bash admin && pw=$(< /dev/urandom tr -dc _A-Z-a-z-0-9 | head -c8) && echo "admin:"$pw | chpasswd && echo "username: admin password: "$pw
 ---> Running in xxxxxxxxxxxx
username: admin password: xxxXXxxx
Removing intermediate container xxxxxxxxxxxx
 ---> xxxxxxxxxxxx
Step 3/3 : CMD jupyterhub -f jupyterhub_config.py
 ---> Running in xxxxxxxxxxxx
Removing intermediate container fed4ea0acaee
 ---> xxxxxxxxxxxx
Successfully built xxxxxxxxxxxx
```
The important line from the output is the following one
where the password for the user admin at local container is set. 
This password should be remembered.
```commandline
username: admin password: xxxXXxxx
```