ARG UBUNTU_VERSION="24.04"


############ dependencies ############
FROM "ubuntu:${UBUNTU_VERSION}"

USER root
SHELL ["/bin/bash", "-c"]

ARG CARLA_API_PATH=/opt/carla/PythonAPI
ARG SCENARIO_RUNNER_PATH=/opt/carla/carla-scenario-runner

# Install essentials
RUN apt-get update && \
    apt-get install -y \
      libpng16-16 \
      libtiff6 \
      libjpeg8 \
      build-essential \
      wget \
      git \
      libxerces-c-dev \
      python3-pip \
    && rm -rf /var/lib/apt/lists/*

RUN pip3 install --upgrade pip
RUN update-alternatives --install /usr/bin/python python /usr/bin/python3 12

# Install carla-scenario-runner
COPY ./carla-scenario-runner $SCENARIO_RUNNER_PATH
RUN pip3 install -r $SCENARIO_RUNNER_PATH/requirements.txt

# Move over PythonAPI
COPY ./artifacts/PythonAPI ${CARLA_API_PATH}

# Recursively install PythonAPI requirements, keep version of first occurrence
RUN cat $(find ${CARLA_API_PATH} -type f -name "requirements.txt") > /tmp/requirements_raw.txt \
    && awk -F '==' '{print $1}' /tmp/requirements_raw.txt | awk '!visited[$1]++' > /tmp/requirements.txt \
    && pip3 install -r /tmp/requirements.txt

# Install CARLA PythonAPI wheel
RUN set -e; \
    pyver=$(python3 -c "import sys; print(f'{sys.version_info.major}{sys.version_info.minor}')"); \
    wheel=$(echo ${CARLA_API_PATH}/carla/dist/*${pyver}*.whl); \
    pip install --no-cache-dir "$wheel"

# Create script that adds API to pythonpath and make .bashrc source it
RUN echo "export PYTHONPATH=\$PYTHONPATH:$CARLA_API_PATH/carla/agents" >> /setup_carla_env.sh; \
    echo "export PYTHONPATH=\$PYTHONPATH:$CARLA_API_PATH/carla" >> /setup_carla_env.sh; \
    echo "export CARLA_API_PATH=$CARLA_API_PATH" >> /setup_carla_env.sh; \
    echo "source /setup_carla_env.sh" >> ~/.bashrc

# Needed for (pygame based) scripts that have a GUI
ENV SDL_VIDEODRIVER=x11

USER root
SHELL ["/bin/bash", "-c"]

ARG WORKSPACE=${SCENARIO_RUNNER_PATH}
ENV WORKSPACE=${WORKSPACE}
WORKDIR ${WORKSPACE}

# Set entrypoint
COPY ./carla-scenario-runner/docker/entrypoint.sh /
ENTRYPOINT [ "/entrypoint.sh" ]

CMD [ "python3", "scenario_runner.py", "--help" ]
