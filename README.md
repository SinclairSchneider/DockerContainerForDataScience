# DockerContainerForDataScience
## Jupyterhub GPU
[jupyterhub_gpu](https://github.com/SinclairSchneider/DockerContainerForDataScience/tree/main/jupyterhub_gpu) offers an Jupyterhub Server alongside with many important python packages for machine learning.
The jupyterhub server runs in the jupyterlab flavour.
The included programs and libraries are:
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
- vim
- htop
- net-tools
- git
- ipywidgets (python 3)
- pandahouse (python 3)
- clickhouse-sqlalchemy (python 3)
- clickhouse-driver (python 3)

![](https://raw.githubusercontent.com/SinclairSchneider/DockerContainerForDataScience/main/jupyterhub_gpu/img/Login_Jupyterhub.jpg)
## Jupyterhub(CUDA) + Apache Superset + Clickhouse Docker image
[data_science_toolkit](https://github.com/SinclairSchneider/DockerContainerForDataScience/tree/main/data_science_toolkit)
offers an Jupyterhub Server, Apache Superset and a Clickhouse database. 
Like the previous container this one also comes with some extra packages.
Since the container inherits from jupyterhub_gpu, all its features are also included.
The Jupyterhub Server is mapped to port 8000, Apache Superset to port 8088 and Clickhouse to port 8123.
The following programs and libraries are included (the bold ones are new compared to jupyterhub_gpu):
- Inheritance from image sinclair1992/jupyterhub_gpu
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
- vim
- htop
- net-tools
- git
- ipywidgets (python 3)
- pandahouse (python 3)
- clickhouse-sqlalchemy (python 3)
- clickhouse-driver (python 3)
- **apt-utils**
- **apt-transport-https**
- **ca-certificates**
- **dirmngr**
- **clickhouse-server**
- **clickhouse-client**
- **build-essential**
- **libssl-dev**
- **libffi-dev**
- **python3-dev**
- **libsasl2-dev**
- **libldap2-dev**
- **apache-superset (python 3)**
- **Flask-WTF==0.14.3 (python 3)**

![](https://raw.githubusercontent.com/SinclairSchneider/DockerContainerForDataScience/main/data_science_toolkit/img/Login_Jupyterhub.jpg)
![](https://raw.githubusercontent.com/SinclairSchneider/DockerContainerForDataScience/main/data_science_toolkit/img/Login_Superset.jpg)
![](https://raw.githubusercontent.com/SinclairSchneider/DockerContainerForDataScience/main/data_science_toolkit/img/Connection_Clickhouse.jpg)