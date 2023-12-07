# Utilize GPU
FROM nvidia/cuda:latest
CMD nvidia-smi

# Use a multi-stage build
FROM debian:latest AS build

# Install dependencies
RUN apt update \
 && apt install -y --no-install-recommends \
    wget \
    git \
    python3 \
    python3.11-venv \
    ca-certificates \
    google-perftools \
    ffmpeg \
    libsm6 \
    libxext6 \
 && rm -rf /var/lib/apt/lists/* \
 && wget "https://letsencrypt.org/certs/isrg-root-x1-cross-signed.pem" --no-check-certificate \
 && mkdir -p /etc/ssl/certs/ \
 && mv isrg-root-x1-cross-signed.pem /etc/ssl/certs/.

# add a non-root user and switch to this user
RUN useradd -ms /bin/bash stablediffuser
USER stablediffuser
WORKDIR /home/stablediffuser

# Do the appropriate actions for the "venv"
RUN [ "/bin/bash", "-c", "python3 -c 'import venv' && python3 -m venv venv/" ]

# Download the repo
RUN git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui.git

# Ensure 0.0.0.0:7860
RUN sed -i 's/\#export\ COMMANDLINE_ARGS/export\ COMMANDLINE_ARGS/g' ./stable-diffusion-webui/webui-user.sh \
 && sed -i 's/COMMANDLINE_ARGS\=\"\"/COMMANDLINE_ARGS\=\"\-\-listen\ \-\-no\-download\-sd\-model\ \-\-xformers\ \-\-opt\-sdp\-attention\ \-\-precision\ full\ \-\-no\-half\ \-\-medvram\ \-\-lowvram\"/g' ./stable-diffusion-webui/webui-user.sh \
 && cp ./stable-diffusion-webui/webui-user.sh ./webui-user.sh

# Run the script
CMD /bin/bash -c "$( wget -qO- https://raw.githubusercontent.com/AUTOMATIC1111/stable-diffusion-webui/master/webui.sh)"
