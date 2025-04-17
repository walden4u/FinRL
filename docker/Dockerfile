FROM stablebaselines/rl-baselines3-zoo

WORKDIR /src

COPY requirements.txt .

# Switch to root user to install packages
USER root

# Install pip and development libraries using apt
RUN apt-get update && apt-get install -y --no-install-recommends wget python3-pip libsdl2-dev libsdl2-image-dev libsdl2-mixer-dev libsdl2-ttf-dev libportmidi-dev python3-dev && apt-get clean && rm -rf /var/lib/apt/lists/*

# 4. 의존성 설치 (pip 사용)
# Let pip handle dependencies from requirements.txt. If pygame 2.1.0 fails again,
# it confirms a hard dependency issue we might need to address in requirements.txt.
RUN python3 -m pip install -r requirements.txt --break-system-packages

RUN python3 -m pip install jupyterlab --break-system-packages

# Add Tini. Tini operates as a process subreaper for jupyter. This prevents kernel crashes.
ENV TINI_VERSION v0.6.0
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /usr/bin/tini
RUN chmod +x /usr/bin/tini

# copy the code instead of mounting
COPY . .

ENTRYPOINT ["/usr/bin/tini", "--"]

CMD ["jupyter", "lab", "--port=8888", "--no-browser", "--ip=0.0.0.0", "--NotebookApp.token=''","--NotebookApp.password=''","--allow-root"]
