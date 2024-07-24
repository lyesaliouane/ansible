FROM debian:latest

ARG DEBIAN_FRONTEND=noninteractive

RUN apt update --allow-insecure-repositories \
&& apt -y install --allow-unauthenticated apt-utils net-tools sudo python3 curl wget vim cron \
&& apt -y install --allow-unauthenticated openssh-server openssh-client supervisor \
&& mkdir -p /mnt/app \
&& mkdir -p /mnt/data


RUN apt update --allow-insecure-repositories \
&& apt install -f --allow-unauthenticated \
&& apt -y autoremove

# Create SSH configuration directory
RUN mkdir -p /var/run/sshd

# Allow root login via SSH
RUN sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config \
&& echo 'root:iotlabs85' | chpasswd

# Add new SSH user
RUN useradd -m lyes \
    && echo 'lyes:iotlabs85' | chpasswd \
    && usermod -aG sudo lyes \
    && mkdir -p /home/lyes/.ssh \
    && ssh-keygen -t rsa -b 4096 -f /home/lyes/.ssh/id_rsa -q -N "" \
    && chown -R lyes:lyes /home/lyes/.ssh


WORKDIR /mnt/app

# Expose SSH port
EXPOSE 22

# Copy supervisor configuration file
COPY supervisor.conf /etc/supervisor/conf.d/supervisor.conf

# Start supervisor
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/supervisord.conf"]