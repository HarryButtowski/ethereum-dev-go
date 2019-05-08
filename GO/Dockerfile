FROM ubuntu:18.04

ADD ./goland-2019.1.1.tar.gz /home/developer
ADD ./go1.12.4.linux-amd64.tar.gz /usr/local

RUN apt-get autoremove && apt-get clean && apt-get autoclean && apt-get -f install
RUN apt-get -y update && apt-get install -y --no-install-recommends sudo \
    default-jre \
    firefox \
    chromium-browser \
    git \
    gcc \
    libc6-dev \
    fuse

RUN export uid=1000 gid=1000 && \
    mkdir -p /home/developer/go/src && \
    echo "developer:x:${uid}:${gid}:Developer,,,:/home/developer:/bin/bash" >> /etc/passwd && \
    echo "developer:x:${uid}:" >> /etc/group && \
    echo "developer ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/developer && \
    chmod 0440 /etc/sudoers.d/developer && \
    chown ${uid}:${gid} -R /home/developer

USER developer

ENV HOME /home/developer
ENV PATH $PATH:/usr/local/go/bin
ENV GOPATH /home/developer/go
ENV GOROOT /usr/local/go

WORKDIR /home/developer

VOLUME /home/developer/go/src

RUN /usr/local/go/bin/go get github.com/ethereum/go-ethereum/ethclient

COPY ./Ganache-2.0.1.AppImage /usr/local/bin
RUN sudo chmod +x /usr/local/bin/Ganache-2.0.1.AppImage

ENTRYPOINT /home/developer/GoLand-2019.1.1/bin/goland.sh
