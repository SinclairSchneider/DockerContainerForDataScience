FROM sinclair1992/jupyterhub_gpu
RUN useradd -mg users -G sudo -s /bin/bash admin && pw=$(< /dev/urandom tr -dc _A-Z-a-z-0-9 | head -c8) && echo "admin:"$pw | chpasswd && echo "username: admin password: "$pw
CMD jupyterhub -f jupyterhub_config.py
