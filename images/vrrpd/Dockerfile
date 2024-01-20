#import from w4sp base
FROM w4sp/labs:base

#install vrrpd, lldpd and quagga
#RUN apt-get install -y vrrpd lldpd quagga
#RUN echo "deb http://deb.debian.org/debian buster main" >> /etc/apt/sources.list
FROM ubuntu:20.04
RUN apt-get update
ENV DEBIAN_FRONTEND=noninteractive
ENV TZ=America/New_York
RUN apt-get install -y vrrpd lldpd quagga


#add the quagga scripts
ADD daemons /etc/quagga/daemons
USER quagga
ADD ospfd.conf /etc/quagga/ospfd.conf
ADD zebra.conf /etc/quagga/zebra.conf
USER root

#add supervisord script and startup script and disable initial dhclient
ADD start_vrrpd.py /start_vrrpd.py
ADD supervisor_vrrpd.conf /etc/supervisor/conf.d/supervisor_vrrpd.conf
RUN rm -f /etc/supervisor/conf.d/supervisor_dhclient.conf
