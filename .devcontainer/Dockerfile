FROM ubuntu:bionic

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Install environment

## Install basic tools
RUN apt-get update && \
    apt-get -y install vim tmux curl wget git build-essential python3-dev cmake gcc shellcheck unzip

## Install latest docker
RUN curl -fsSL https://get.docker.com | bash

## Install latest docker-compose
RUN DOCKER_COMPOSE_VERSION="$(git ls-remote https://github.com/docker/compose | grep refs/tags | grep -oE "[0-9]+\.[0-9]+\.[0-9]+$" | sort --version-sort | tail -n 1)" && \
    curl -fSsL "https://github.com/docker/compose/releases/download/${DOCKER_COMPOSE_VERSION}/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose && \
    chmod +x /usr/local/bin/docker-compose

## Install latest docker-lock
RUN DOCKER_LOCK_VERSION="$(git ls-remote https://github.com/michaelperel/docker-lock | grep refs/tags | grep -oE "[0-9]+\.[0-9]+\.[0-9]+$" | sort --version-sort | tail -n 1)" && \
    mkdir -p "${HOME}/.docker/cli-plugins" && \
    curl -fsSL "https://github.com/michaelperel/docker-lock/releases/download/v${DOCKER_LOCK_VERSION}/docker-lock-linux" -o "${HOME}/.docker/cli-plugins/docker-lock" && \
    chmod +x "${HOME}/.docker/cli-plugins/docker-lock"

## Install latest Azure CLI
RUN curl -fSsL https://aka.ms/InstallAzureCLIDeb | bash

## Install latest kubectl
RUN KUBECTL_VERSION="$(curl -fSsL https://storage.googleapis.com/kubernetes-release/release/stable.txt)" && \
    curl -fSsL "https://storage.googleapis.com/kubernetes-release/release/${KUBECTL_VERSION}/bin/linux/amd64/kubectl" -o /usr/local/bin/kubectl && \
    chmod +x /usr/local/bin/kubectl

## Install latest helm
RUN curl -fSsL https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash

## Install latest fluxctl
RUN FLUXCTL_VERSION="$(git ls-remote https://github.com/fluxcd/flux | grep refs/tags | grep -oE "[0-9]+\.[0-9]+\.[0-9]+$" | sort --version-sort | tail -n 1)" && \
    curl -fSsL "https://github.com/fluxcd/flux/releases/download/${FLUXCTL_VERSION}/fluxctl_linux_amd64" -o /usr/local/bin/fluxctl && \
    chmod +x /usr/local/bin/fluxctl

## Install latest kubeseal
RUN KUBESEAL_VERSION="$(git ls-remote https://github.com/bitnami-labs/sealed-secrets | grep refs/tags | grep -oE "[0-9]+\.[0-9]+\.[0-9]+$" | sort --version-sort | tail -n 1)" && \
    curl -fSsL "https://github.com/bitnami-labs/sealed-secrets/releases/download/v${KUBESEAL_VERSION}/kubeseal-linux-amd64" -o /usr/local/bin/kubeseal && \
    chmod +x /usr/local/bin/kubeseal

## Install latest terraform
RUN TERRAFORM_VERSION="$(curl -fSsL https://releases.hashicorp.com/terraform/ | grep -oE "[0-9]+\.[0-9]+\.[0-9]+" | head -n 1)" && \
    curl -fSsLO "https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_linux_amd64.zip" && \
    unzip "terraform_${TERRAFORM_VERSION}_linux_amd64.zip" && \
    mv terraform /usr/local/bin && \
    chmod +x /usr/local/bin/terraform

## Install latest Anaconda Python
RUN ANACONDA_VERSION="$(curl https://repo.anaconda.com/archive/ | grep -oE "[0-9]{4}\.[0-9]{2}" | head -n 1)" && \
    ANACONDA_SCRIPT="Anaconda3-${ANACONDA_VERSION}-$(uname -s)-$(uname -m).sh" && \
    curl -fsSLO "https://repo.anaconda.com/archive/${ANACONDA_SCRIPT}" && \
    bash "${ANACONDA_SCRIPT}" -b -p "/usr/local/anaconda3" && \
    rm "${ANACONDA_SCRIPT}"

## Install latest go
RUN GO_VERSION="$(git ls-remote https://github.com/golang/go | grep refs/tags | grep -oE "[0-9]+\.[0-9]+\.[0-9]+$" | sort --version-sort | tail -n 1)" && \
    curl -fSsL "https://dl.google.com/go/go${GO_VERSION}.linux-amd64.tar.gz" | tar -xz -C /usr/local

## Set PATH
RUN echo "export PATH=/usr/local/anaconda3/bin:/usr/local/go/bin:${PATH}" >> "${HOME}/.bashrc"
ENV PATH="/usr/local/anaconda3/bin:/usr/local/go/bin:${PATH}"

# Customize environment

## Install vim-awesome basic edition
RUN curl -fSsL https://raw.githubusercontent.com/amix/vimrc/master/vimrcs/basic.vim > "${HOME}/.vimrc"

## Add custom vimrc
COPY custom_vimrc .
RUN cat custom_vimrc >> "${HOME}/.vimrc" && \
    rm custom_vimrc

## Install vim-plug
RUN curl -fSsLo "${HOME}/.vim/autoload/plug.vim" --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim

## Install vim plugins
RUN vim +'PlugInstall --sync' +qa > /dev/null

## Install python dependencies
COPY requirements.txt .
RUN pip install -r requirements.txt && \
    rm requirements.txt

# Cleanup
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /workspaces/app
