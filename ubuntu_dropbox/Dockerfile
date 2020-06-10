FROM ubuntu:20.04
ENV DEBIAN_FRONTEND=noninteractive
RUN apt update
RUN apt install -y libgtk2.0-dev libglapi-mesa libxcb-glx0 libxcb-dri2-0 libxcb-dri3-0 libxcb-present0 libxcb-sync1 libxshmfence1 libxxf86vm1 wget
RUN wget -O - "https://www.dropbox.com/download?plat=lnx.x86_64" | tar xzf -
