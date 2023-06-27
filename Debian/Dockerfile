FROM kasmweb/core-debian-bullseye:1.13.1
USER root

ENV HOME /home/kasm-default-profile
ENV STARTUPDIR /dockerstartup
ENV INST_SCRIPTS $STARTUPDIR/install
WORKDIR $HOME

######### Customize Container Here ###########

### Install Tailscale and Firefox-ESR ###

RUN curl -fsSL https://pkgs.tailscale.com/stable/debian/bullseye.noarmor.gpg | tee /usr/share/keyrings/tailscale-archive-keyring.gpg >/dev/null \
    && curl -fsSL https://pkgs.tailscale.com/stable/debian/bullseye.tailscale-keyring.list | tee /etc/apt/sources.list.d/tailscale.list \
    && apt update \
    && apt install -y tailscale firefox-esr iputils-ping \
    && cp /usr/share/applications/firefox-esr.desktop $HOME/Desktop/ \
    && chmod +x $HOME/Desktop/firefox-esr.desktop \
    && chown 1000:1000 $HOME/Desktop/firefox-esr.desktop
    
### Install VSCode ###    
    
RUN apt install -y wget gpg \
    && wget -qO - https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg \
    && install -D -o root -g root -m 644 packages.microsoft.gpg /etc/apt/keyrings/packages.microsoft.gpg \
    && sh -c 'echo "deb [arch=amd64,arm64,armhf signed-by=/etc/apt/keyrings/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" > /etc/apt/sources.list.d/vscode.list' \
    && rm -f packages.microsoft.gpg \
    && apt install -y apt-transport-https \
    && apt update \
    && apt install -y code \
    && cp /usr/share/applications/code.desktop $HOME/Desktop/ \
    && chmod +x $HOME/Desktop/code.desktop \
    && chown 1000:1000 $HOME/Desktop/code.desktop
    
### Install sudo and add default user to sudo ###
    
RUN apt update \
    && apt install -y sudo \
    && echo 'kasm-user ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers \
    && rm -rf /var/lib/apt/list/*

######### End Customizations ###########

RUN chown 1000:0 $HOME
RUN $STARTUPDIR/set_user_permission.sh $HOME

ENV HOME /home/kasm-user
WORKDIR $HOME
RUN mkdir -p $HOME && chown -R 1000:0 $HOME

USER 1000