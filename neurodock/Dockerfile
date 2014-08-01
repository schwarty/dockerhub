FROM ubuntu:14.04

MAINTAINER schwarty

# Environment variables
ENV TERM linux

# Update repository
RUN apt-get update
RUN apt-get upgrade -y

# Install packages
RUN apt-get install -y git-core build-essential wget ssh-client openssh-server
RUN apt-get install -y python2.7-dev
RUN apt-get install -y python-numpy
RUN apt-get install -y python-scipy
RUN apt-get install -y python-pip
RUN apt-get install -y python-zmq
RUN apt-get install -y python-jinja2
RUN apt-get install -y python-tornado
RUN apt-get install -y python-sympy
RUN apt-get install -y libfreetype6-dev

# Pip installs
RUN pip install jsonschema
RUN pip install jsonpointer

# Install Python libraries from source

RUN echo "cloning..."

WORKDIR /

RUN mkdir build
WORKDIR build

RUN git clone https://github.com/matplotlib/matplotlib.git
RUN git clone https://github.com/scikit-learn/scikit-learn.git
RUN git clone https://github.com/nipy/nibabel.git
RUN git clone https://github.com/nipy/nipy.git
RUN git clone https://github.com/nilearn/nilearn.git
RUN git clone https://github.com/joblib/joblib.git
RUN git clone https://github.com/ipython/ipython.git

RUN echo "building..."

WORKDIR /build/matplotlib
RUN python setup.py install

WORKDIR /build/scikit-learn
RUN python setup.py install

WORKDIR /build/nibabel
RUN python setup.py install

WORKDIR /build/nipy
RUN python setup.py install

WORKDIR /build/nilearn
RUN python setup.py install

WORKDIR /build/joblib
RUN python setup.py install

WORKDIR /build/ipython
RUN python setup.py install

# Install SPM
ENV SPM_DIR /usr/local/src/spm8-standalone-5236/spm8_mcr/spm8
ENV SPM_MCR /usr/local/bin/spm8

ADD spm8 /usr/local/bin/
WORKDIR /usr/local/src
RUN wget https://www.dropbox.com/s/gen1kk1izotp3d3/spm8.tgz
RUN tar xzf /usr/local/src/spm8.tgz
RUN chmod 777 /usr/local/bin/spm8
RUN chmod 777 /usr/local/src/spm8-standalone-5236/run_spm8.sh
RUN chmod 777 /usr/local/src/spm8-standalone-5236/spm8_glnxa64

# Neurodebian

RUN wget -O- http://neuro.debian.net/lists/trusty.de-m.full | tee /etc/apt/sources.list.d/neurodebian.sources.list
RUN apt-key adv --recv-keys --keyserver pgp.mit.edu 2649A5A9
RUN apt-get update -y
RUN apt-get install -y fsl
# RUN source /etc/fsl/5.0/fsl.sh

# Cleanup
WORKDIR /
RUN rm -rf /build
RUN rm -rf /usr/local/src/spm8.tgz
RUN apt-get autoremove -y
RUN apt-get clean all