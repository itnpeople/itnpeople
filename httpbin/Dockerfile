FROM kennethreitz/httpbin:latest

RUN apt-get update
RUN apt-get install curl iputils-ping -y 
RUN apt-get install -y dnsutils iproute2 net-tools traceroute netcat nmap wget git vim iperf3
RUN wget https://github.com/fortio/fortio/releases/download/v1.4.2/fortio-linux_x64-1.4.2.tgz && \
	tar -vxzf fortio-linux_x64-1.4.2.tgz && \
	rm fortio-linux_x64-1.4.2.tgz
RUN curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl" && \
	chmod +x kubectl && \
	mv kubectl /usr/bin/kubectl
