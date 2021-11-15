# Jupyterhub(CUDA) + Apache Superset + Clickhouse Docker image

The following Dockerfiles are based on the Docker image [jupyterhub_gpu](https://github.com/SinclairSchneider/DockerContainerForDataScience/tree/main/jupyterhub_gpu)
and extend these image by the business intelligence application [Apache Superset](https://superset.apache.org/) and the database [Clickhouse](https://clickhouse.com/).
The Jupyterhub server instance is on port 8000, Apache uperset uses port 8088
and the database Clickhouse is accessible through port 8123. 
These ports can be mapped to any convenient ports on the host machine. 
Like with the image [jupyterhub_gpu](https://github.com/SinclairSchneider/DockerContainerForDataScience/tree/main/jupyterhub_gpu)
this folder also contains two subfolders "data_science_toolkit" and "data_science_toolkit_local".
Again each of these folders has its own Dockerfile in it. 
The Dockerfile inside the "data_science_toolkit" folder has the full building plan for an environment with the following three components:
Jupyterhub-Server, Apache Superset and Clickhouse along with some extras. 
The Dockerfile inside the folder "data_science_toolkit_local" only contains the finalization steps such as setting a password or creating folders.

The image comes with the following properties (the bold ones are new compared to [jupyterhub_gpu](https://github.com/SinclairSchneider/DockerContainerForDataScience/tree/main/jupyterhub_gpu)):
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
- **apt-utils**
- **apt-transport-https**
- **ca-certificates**
- **dirmngr**
- **clickhouse-server**
- **clickhouse-client**
- **git**
- **build-essential**
- **libssl-dev**
- **libffi-dev**
- **python3-dev**
- **libsasl2-dev**
- **libldap2-dev**
- **apache-superset (python 3)**
- **Flask-WTF==0.14.3 (python 3)**

## To just run the container
To just run the container, clone this git repository and got to the directory
"data_science_toolkit/data_science_toolkit_local/" by the following command:
```commandline
git clone https://github.com/SinclairSchneider/DockerContainerForDataScience
cd DockerContainerForDataScience/data_science_toolkit/data_science_toolkit_local/
```
At the first run the image needs to be initialized. Do this with the following command.
```commandline
docker image build -t sinclair1992/data_science_toolkit_local .
```
The response then might look as follows
```commandline
Sending build context to Docker daemon   2.56kB
Step 1/3 : FROM sinclair1992/data_science_toolkit
latest: Pulling from sinclair1992/data_science_toolkit
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
xxxxxxxxxxxx: Already exists
xxxxxxxxxxxx: Pull complete
xxxxxxxxxxxx: Pull complete
Digest: sha256:eb0c458d2a98f88fc151fde761d31a85de88023011412ecf06d6b94e344a3b7c
Status: Downloaded newer image for sinclair1992/data_science_toolkit:latest
 ---> xxxxxxxxxxxx
Step 2/3 : RUN useradd -mg users -G sudo -s /bin/bash admin && pw=$(< /dev/urandom tr -dc _A-Z-a-z-0-9 | head -c8) && echo "admin:"$pw | chpasswd && sed -i 's/<password><\/password<password>'$pw'<\/password>/g' /etc/clickhouse-server/users.xml && superset fab reset-password --username admin --password $pw && echo "Jupyterhub:" && echo "username: admin" && ec "password: "$pw && echo "Apache Superset:" && echo "username: admin" && echo "password: "$pw && echo "Clickhouse:" && echo "username: default" && echo "password: "$pw
 ---> Running in xxxxxxxxxxxx
logging was configured successfully
2021-11-15 23:56:09,582:INFO:superset.utils.logging_configurator:logging was configured successfully
2021-11-15 23:56:09,588:INFO:root:Configured event logger of type <class 'superset.utils.log.DBEventLogger'>
User admin reseted.
/usr/local/lib/python3.8/dist-packages/flask_caching/__init__.py:201: UserWarning: Flask-Caching: CACHE_TYPE is set to null, caching is effectively disabled.
  warnings.warn(
Jupyterhub:
username: admin
password: xxxxxxxx
Apache Superset:
username: admin
password: xxxxxxxx
Clickhouse:
username: default
password: xxxxxxxx
Removing intermediate container xxxxxxxxxxx
 ---> f7ab3862a9fa
Step 3/3 : CMD mkdir -p /home/data/clickhouse/log/clickhouse-server && mkdir -p /home/data/clickhouse/tmp && mkdir -p /home/data/clickhouse/user_files && mkdir -p /home/data/clickhse/access && mkdir -p /home/data/clickhouse/format_schemas && chown -R clickhouse:clickhouse /home/data/clickhouse && clickhouse start && superset run -p 8088 --host=0.0.0.0 --withhreads --reload & jupyterhub -f jupyterhub_config.py & tail -f /dev/null
 ---> Running in xxxxxxxxxxx
Removing intermediate container xxxxxxxxxxx
 ---> xxxxxxxxxxx
Successfully built xxxxxxxxxxx
Successfully tagged sinclair1992/data_science_toolkit_local:latest
```
The important lines from the output are the ones where the passwords for the user accounts for
Jupyterhub, Apache Superset and Clickhouse are set.
All these passwords are the same and randomly generated. 
They should be remembered as they are not shown again when the container is run.
```commandline
Jupyterhub:
username: admin
password: xxxxxxxx
Apache Superset:
username: admin
password: xxxxxxxx
Clickhouse:
username: default
password: xxxxxxxx
```
To run the new created container, the following command needs to be run
```commandline
docker run --cap-add=SYS_NICE --cap-add=NET_ADMIN --cap-add=IPC_LOCK -p 8000:8000 -p 8123:8123 -p 8088:8088 --gpus '"device=0,1"' -d -v /path/to/local/home:/home sinclair1992/data_science_toolkit_local
```
This maps port 8000 inside the docker container to port 8000 of the host machine for the Jupyterhub Server.
The same goes with port 8088 for Apache Superset and port 8123 for clickhouse.
The GPUs with the ids 0 and 1 get attached to the container. 
In case more GPUs are available and needed the numbers can be adjusted accordingly.
The parameter -d means that the container runs in detached mode and -v maps the volume /home
inside the docker container to a local directory.
The arguments --cap-add=SYS_NICE --cap-add=NET_ADMIN --cap-add=IPC_LOCK are needed in order to run the
Clickhouse database according to [this](https://github.com/ClickHouse/ClickHouse/issues/13726) github discussion.
Since the home directory is mounted locally, all user data of the admin are saved even after the docker container is removed.
Clickhouse is configured in a way that all data are stored under "/home/data/clickhouse" which means that they are also 
saved if the container is deleted.
To check if to container is running the following command might be used:
```commandline
docker container ls
```
The following command can then stop the container again:
```commandline
docker container stop [container id]
```
It might be worth to mention that only the first digits of the container id need to be written that are needed to differentiate the container.
Finally a login to the Jupyterhub server is possible either by directly accessing the server on port 8000
or by tunneling the port through ssh to 127.0.0.1 of the local machine.
![](https://raw.githubusercontent.com/SinclairSchneider/DockerContainerForDataScience/main/jupyterhub_gpu/img/Login_Jupyterhub.jpg)
![](https://raw.githubusercontent.com/SinclairSchneider/DockerContainerForDataScience/main/jupyterhub_gpu/img/Putty_port_forwarding.jpg)

## To build the container

To build and edit the main docker container please first change to the directory "DockerContainerForDataScience/jupyterhub_gpu/jupyterhub_gpu"
```commandline
cd DockerContainerForDataScience/jupyterhub_gpu/jupyterhub_gpu
```
The Dockerfile can then be edited before the image is build. 
The building process has a tag parameter which is usually in the form [accountname]/[imagename]
if the image should be uploaded to docker. The command might look like this:
```commandline
docker image build -t sinclair1992/jupyterhub_gpu .
```
After the image has deen build it can finally be pushed after logging in to docker.
The name of the account as well as the name of the image are handed over to the push command.
```commandline
docker login
sudo docker push sinclair1992/jupyterhub_gpu
```
The image has now been build and can be used. When doing so the names of the account and image in the Dockerfile inside the jupyterhub_gpu_local folder
have to be adjusted accordingly.