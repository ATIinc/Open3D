FROM mcr.microsoft.com/devcontainers/cpp:ubuntu-24.04

# Avoid warnings by switching to noninteractive
ENV DEBIAN_FRONTEND=noninteractive

# Install pip and python-is-python3 to make 'python' alias to 'python3'
RUN apt-get update && apt-get install -y \
    python3 \
    python3-pip \
    python-is-python3 \
    python3-venv

# Copy install script
COPY util/install_deps_ubuntu.sh /tmp/install_deps_ubuntu.sh

# Run install script
# The script uses sudo, but we are root here, so define SUDO=""
RUN chmod +x /tmp/install_deps_ubuntu.sh \
    && SUDO="" /tmp/install_deps_ubuntu.sh assume-yes

# Install python requirements
COPY python/requirements.txt /tmp/requirements.txt
COPY python/requirements_build.txt /tmp/requirements_build.txt
COPY python/requirements_test.txt /tmp/requirements_test.txt
RUN python3 -m venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"
RUN pip3 install -r /tmp/requirements.txt -r /tmp/requirements_build.txt -r /tmp/requirements_test.txt

# Clean up
RUN rm /tmp/install_deps_ubuntu.sh /tmp/requirements.txt /tmp/requirements_build.txt /tmp/requirements_test.txt
