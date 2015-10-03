# CREATE: docker build --rm=true -t "vasian/gui" .
# START: docker run --rm -P -it --name temp1 -u user vasian/gui
# CONNECT: vncviewer localhost:$(docker port temp1 5900 | cut -d : -f 2)

FROM ubuntu:14.04

ENV DEBIAN_FRONTEND noninteractive

RUN locale-gen --no-purge en_US.UTF-8
ENV LC_ALL en_US.UTF-8
RUN apt-get update

# not really needed
RUN apt-get -y install fuse || true
RUN rm -rf /var/lib/dpkg/info/fuse.postinst
RUN apt-get -y install fuse

# gui, browser, editor and terminal
RUN apt-get install -y xvfb x11vnc openbox firefox nano xterm

# to make the final image smaller
RUN apt-get clean

# add user
RUN useradd -m -d /home/user user
RUN echo "user:password" | chpasswd
RUN adduser user sudo
RUN chsh -s /bin/bash user

#vnc
RUN mkdir /home/user/.vnc
RUN x11vnc -storepasswd password home/user/.vnc/passwd
RUN chown -R user /home/user/.vnc
RUN chgrp -R user /home/user/.vnc
EXPOSE 5900

ADD startXvfb.sh /startXvfb.sh

ENV HOME /home/user
WORKDIR /home/user
ENV DISPLAY :0

# not working
#USER user

ENTRYPOINT /startXvfb.sh xterm firefox
