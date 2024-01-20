#import from w4sp base
FROM w4sp/labs:base

#install samba
FROM ubuntu:20.04
RUN apt-get install -y samba

RUN apt-get update
FROM ubuntu:latest

# Create a Samba share
RUN mkdir /samba/share

# Configure Samba
COPY smb.conf /etc/samba/smb.conf

# Start Samba
CMD ["smbd", "-s", "/etc/samba/smb.conf"]
#set a samba password for the w4sp user to w4spbook
# Set the password as an environment variable
 
#Use the echo command to provide the password to smbpasswd
#RUN echo -e "${PASS}\n${PASS}" | smbpasswd -a -s studio45
RUN pdbedit -L -w
#RUN service smbd status
RUN smbd --version
RUN whereis smbd
# Echo Samba version
RUN echo "Samba version: $(smbd --version)"
#RUN printf "studio45\nstudio45" | smbpasswd -a -s w4sp


FROM ubuntu:latest

# Set environment variables
ENV USERNAME=w4sp
ENV PASSWORD=studio45

# Install necessary utilities
RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y sudo && \
    rm -rf /var/lib/apt/lists/*

# Add a user and set a password
RUN useradd -m -s /bin/bash "$USERNAME" && \
    echo "$USERNAME:$PASSWORD" | chpasswd && \
    usermod -aG sudo "$USERNAME"

# Switch to the new user
USER $USERNAME

# Set the working directory
WORKDIR /home/$USERNAME

# Display the username for verification
CMD echo "User: $USERNAME"

ARG SMB_USER=w4sp
ARG SMB_PASSWORD=studio45
ARG SMB_UID=1000  # Set the desired UID for the Samba us
#RUN echo ${PASS} | tee - | smbpasswd -a -s w4sp

# Switch to root to add user to a group
USER root
#add the smbgrp group and add w4sp to the group
RUN addgroup smbgrp && usermod -a -G smbgrp w4sp
# Switch back to the regular user
USER $USERNAME
USER root
#create the file share directories
RUN mkdir -p /file_shares/anon && mkdir -p /file_shares/secured

#set perms for anonymous share
RUN chmod -R 0755 /file_shares/anon && chown -R nobody:nogroup /file_shares/anon
#set perms for w4sp user
RUN chmod -R 0755 /file_shares/secured && chown -R w4sp:smbgrp /file_shares/secured
USER $USERNAME
#add configs
ADD smb.conf /etc/samba/smb.conf
ADD supervisor_samba.conf /etc/supervisor/conf.d/supervisor_samba.conf

#going to create some files
RUN /bin/bash -c "echo 'super secret' >> /file_shares/secured/secret.txt"
RUN /bin/bash -c "echo 'bar' >> /file_shares/secured/tmp.txt"


