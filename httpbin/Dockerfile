FROM kennethreitz/httpbin:latest

RUN apt-get update
RUN apt-get install curl iputils-ping -y 
RUN apt-get install -y dnsutils
RUN apt-get install -y iproute2 net-tools traceroute
RUN apt-get install -y netcat nmap
RUN apt-get install -y wget git vim
RUN apt-get install -y iperf3
RUN wget https://github.com/fortio/fortio/releases/download/v1.4.2/fortio-linux_x64-1.4.2.tgz
RUN tar -vxzf fortio-linux_x64-1.4.2.tgz