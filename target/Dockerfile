FROM ubuntu:22.04
RUN apt-get update && apt-get install -y openssh-server python3 sudo \
    && mkdir -p /run/sshd && ssh-keygen -A
RUN useradd -m -s /bin/bash ansible \
    && echo 'ansible ALL=(ALL) NOPASSWD:ALL' > /etc/sudoers.d/ansible
COPY authorized_keys /home/ansible/.ssh/authorized_keys
RUN chown -R ansible:ansible /home/ansible/.ssh \
    && chmod 700 /home/ansible/.ssh \
    && chmod 600 /home/ansible/.ssh/authorized_keys
CMD ["/usr/sbin/sshd", "-D"]