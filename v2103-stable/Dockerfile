# Build Kobocoind and kobocoin-qt from the code at
# https://github.com/kobocoin/Kobocoin/commit/daa6a39d7485bfe5fc7efb77ef8acb4ff92bb7db

FROM ubuntu:16.04
LABEL maintainer Kobocoin Dev <dev@kobocoin.com>


# 1. Install the minimal requirements to clone the source code and make sure permissions are correct.
RUN apt-get update \

&& apt-get install -y --no-install-recommends \
ca-certificates \
git \

&& cd /opt \
&& git clone https://github.com/kobocoin/Kobocoin \                
&& cd Kobocoin \
&& git checkout daa6a3 \
&& cd .. \
&& chmod -R 775 Kobocoin \
&& cd Kobocoin \
&& apt-get clean


# 2. Install Kobocoin essentials.
RUN apt-get update \

&& apt-get install -y --no-install-recommends \
    build-essential \
    libssl-dev \
    libdb++-dev \
    libgmp3-dev \
    libmpfr-dev \
    libtool \
    openssl \
    pkg-config \
    libminiupnpc-dev \
    libqrencode-dev \


# 3. Install these packages in order to view the QT GUI using a VNC viewer. Other methods are available but all require at least xvfb.
    x11vnc \
    xvfb \

&& apt-get clean


# 4. Install boost libraries (1.58)
RUN apt-get install -y --no-install-recommends libboost-all-dev


# 5.Now build it
WORKDIR /opt/Kobocoin/src

RUN make -f makefile.unix USE_UPNP=- \
&& strip Kobocoind

# Kobocoind should be useable
# ./Kobocoind -daemon -rpcuser=user -rpcpassword=password


# 6. Now install qt5 and build the gui
RUN apt-get install -y --no-install-recommends qt5-default qt5-qmake
# Previous - RUN apt-get install -y --no-install-recommends libqt4-dev qt4-qmake

WORKDIR /opt/Kobocoin
RUN qmake \
&& make \
&& strip kobocoin-qt

COPY entrypoint.sh /
RUN chmod 755 /entrypoint.sh
&& printf "QT_X11_NO_MITSHM=1\nDISPLAY=:0\n" >> /etc/environment
