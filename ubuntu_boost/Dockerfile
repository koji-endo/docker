FROM ubuntu
RUN apt update; apt install -y git  libboost-all-dev tree sudo python3-pip software-properties-common
RUN pip3 install numpy pynvim
RUN add-apt-repository -y ppa:neovim-ppa/stable && apt update && apt -y install neovim
CMD sleep infinity
