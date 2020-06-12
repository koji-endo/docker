FROM centos:centos6.9
WORKDIR /root
RUN yum install -y gcc gcc-c++ zlib-devel bzip2 bzip2-devel readline readline-devel sqlite sqlite-devel openssl openssl-devel git patch wget openmpi openmpi-devel mpich2 mpich2-devel xorg-x11-apps ImageMagick ImageMagick-devel && yum clean all
ENV HOME /root
RUN git clone https://github.com/pyenv/pyenv.git ~/.pyenv
ENV PATH /usr/lib64/mpich/bin:$HOME/.pyenv/bin:$PATH
RUN pyenv install 2.7.16
RUN pyenv global 2.7.16 && pyenv rehash
RUN /root/.pyenv/shims/pip install numpy matplotlib mpi4py jupyter
RUN echo "nameserver 8.8.8.8" >> /etc/resolv.conf
RUN cat /etc/resolv.conf
RUN wget https://neuron.yale.edu/ftp/neuron/versions/v7.5/nrn-7.5.tar.gz
RUN tar xvf nrn-7.5.tar.gz
RUN rm nrn-7.5.tar.gz
WORKDIR /root/nrn-7.5
RUN ./configure --without-iv --prefix=`pwd` --with-paranrn --with-nrnpython=/root/.pyenv/shims/python && make && make install
WORKDIR /root
ENV PATH $PATH:$HOME/nrn7.5/x86_64/bin
RUN cd && git clone https://github.com/Hayato-t/pythonNeuronSimulation.git
COPY .bashrc .
COPY .bash_profile .
COPY meta.json /root/pythonNeuronSimulation/meta.json
CMD ["/bin/sleep","infinity"]
