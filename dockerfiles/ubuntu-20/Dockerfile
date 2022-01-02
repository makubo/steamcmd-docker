######## INSTALL ########

# Set the base image
FROM ubuntu:20.04

# Insert Steam prompt answers
SHELL ["/bin/bash", "-o", "pipefail", "-c"]
RUN echo steam steam/question select "I AGREE" | debconf-set-selections \
 && echo steam steam/license note '' | debconf-set-selections

# Update the repository and install SteamCMD
ARG DEBIAN_FRONTEND=noninteractive
RUN dpkg --add-architecture i386 \
 && apt-get update -y \
 && apt-get install -y --no-install-recommends ca-certificates locales steamcmd \
 && rm -rf /var/lib/apt/lists/*

# Create symlink for executable
RUN ln -s /usr/games/steamcmd /usr/bin/steamcmd

# Add unicode support
RUN locale-gen en_US.UTF-8

# Prepare steamcmd user
RUN useradd -md /home/steam -s /bin/bash steam

# Switch user
#USER steam:steam
#WORKDIR /home/steam

# Set Locale variables
ENV LANG 'en_US.UTF-8'
ENV LANGUAGE 'en_US:en'

# Update SteamCMD and verify latest version
RUN su steam -c "steamcmd +quit"

# Set default command
ENTRYPOINT ["su", "steam", "-c"]
CMD ["steamcmd +help +quit"]
