FROM kasmweb/core-debian-bullseye:1.13.1
USER root

ENV HOME /home/kasm-default-profile
ENV STARTUPDIR /dockerstartup
ENV INST_SCRIPTS $STARTUPDIR/install
WORKDIR $HOME

######### Customize Container Here ###########

### Install KDE Plasma ###

RUN apt update \
    && apt purge -y xfce* lightdm \
    && DEBIAN_FRONTEND=noninteractive apt install -y --no-install-recommends chromium sddm plasma-desktop plasma-workspace \
    # && rm $HOME/.vnc/xstartup \
    && mkdir $HOME/.vnc \
    && echo "#!/bin/sh" > $HOME/.vnc/xstartup \
    && echo "/usr/bin/startplasma-x11 &" >> $HOME/.vnc/xstartup \
    && chmod +x $HOME/.vnc/xstartup

### Install Tailscale, Firefox-ESR, VLC, Ping ###


    
### Install sudo and add default user to sudo ###
    
RUN apt update \
    && apt install -y sudo \
    && echo 'kasm-user ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers \
    && rm -rf /var/lib/apt/list/*
    
### Cleanup ###
RUN apt autoclean \
    && rm -rf /config/.cache /var/lib/apt/lists/* /var/tmp/* /tmp/*
    
ENV START_XFCE4=0

######### End Customizations ###########

RUN chown 1000:0 $HOME
RUN $STARTUPDIR/set_user_permission.sh $HOME

ENV HOME /home/kasm-user
WORKDIR $HOME
RUN mkdir -p $HOME && chown -R 1000:0 $HOME

USER 1000