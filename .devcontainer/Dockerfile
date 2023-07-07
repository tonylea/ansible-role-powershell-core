# You can pick any Debian/Ubuntu-based image. 😊
FROM mcr.microsoft.com/devcontainers/base:bullseye

COPY library-scripts/*.sh /tmp/library-scripts/

ARG INSTALL_ZSH=true
ARG UPGRADE_PACKAGES=true
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID
ARG NODE_VERSION="lts/*"

ENV NVM_DIR=/usr/local/share/nvm
ENV NVM_SYMLINK_CURRENT=true \
  PATH=${NVM_DIR}/current/bin:${PATH}

RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
  # Install common dependencies
  && bash /tmp/library-scripts/common-debian.sh "${INSTALL_ZSH}" "${USERNAME}" "${USER_UID}" "${USER_GID}" "${UPGRADE_PACKAGES}" "true" "true" \
  # Install Azure CLI
  && bash /tmp/library-scripts/azcli-debian.sh \
  # Install Docker 
  && bash /tmp/library-scripts/docker-debian.sh "true" "/var/run/docker-host.sock" "/var/run/docker.sock" "${USERNAME}" \
  # Install Node
  && bash /tmp/library-scripts/node-debian.sh "${NVM_DIR}" "${NODE_VERSION}" "${USERNAME}" \
  # Install packages
  && apt-get install -y libssl-dev libffi-dev python3-dev python3-pip \
  # Clean up
  && apt-get clean -y && rm -rf /var/lib/apt/lists/*

# Install Ansible
RUN pip3 install \
  ansible[azure] \
  molecule \
  ansible-lint \
  yamllint \
  molecule-plugins[azure] \
  molecule-plugins[docker] \
  molecule-plugins[vagrant] \
  && pip3 install --upgrade --user setuptools

ENTRYPOINT [ "/usr/local/share/docker-init.sh" ]
CMD [ "sleep", "infinity" ]