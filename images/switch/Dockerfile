#import from our base image
FROM w4sp/labs:base

#RUN apt-get install -y dnsmasq software-properties-common
RUN apt-get update && \
    apt-cache search dnsmasq && \
    apt-cache search software-properties-common && \
    apt-get install -y dnsmasq software-properties-common


#add suricata and logstash repos
#RUN add-apt-repository -y ppa:oisf/suricata-stable
FROM ubuntu:20.04
RUN apt-get update && \
    apt-get install -y gnupg && \
    apt-get clean

RUN apt-get update && \
    apt-get install -y software-properties-common

RUN add-apt-repository ppa:oisf/suricata-stable
RUN apt-get install -y suricata
RUN systemctl enable suricata.service

#RUN wget -qO - https://packages.elastic.co/GPG-KEY-elasticsearch |  apt-key add -
#RUN echo "deb http://packages.elastic.co/logstash/2.0/debian stable main" | tee -a /etc/apt/sources.list
#RUN apt-get update
# Continue with your Dockerfile
# Continue with your Dockerfile
RUN wget -qO - https://packages.elastic.co/GPG-KEY-elasticsearch | gpg --dearmor - | tee /etc/apt/trusted.gpg.d/elastic.gpg > /dev/null

RUN apt-get update



#install suricata and logstah
#RUN apt-get install -y suricata logstash openjdk-7-jre-headless
RUN apt-get install -y suricata
RUN apt-get install -y openjdk-11-jre-headless
RUN echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | tee -a /etc/apt/sources.list.d/elastic-7.x.list
RUN apt-get update
RUN apt-get install -y logstash
ADD logstash.conf /etc/logstash/conf.d/logstash.conf
ADD suricata.yaml /etc/suricata/suricata.yaml
RUN touch /var/log/suricata/eve.json

#install lldp
RUN apt-get -y install lldpd

#add the startup script to add nics to the bridge automagically
ADD start_sw.py /start_sw.py

#add supervisord script and remove the dhclient one
ADD supervisor_sw.conf /etc/supervisor/conf.d/supervisor_sw.conf
RUN rm -f /etc/supervisor/conf.d/supervisor_dhclient.conf
