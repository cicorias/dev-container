# What is this?
* A container with the latest version of common tools for development

# What is included
* docker
* docker-compose
* docker-lock
* az cli
* kubectl
* helm
* fluxctl
* terraform
* anaconda 3 distribution of python
* go
* vim with a sensible configuration

# Usage
* The container is a few gigabytes. It prioritizes readability and breadth of tools over size.
* To use a recently built version:

```
docker pull mperel/dev-container
```

* To build from scratch:

```
docker build -f .devcontainer/Dockerfile -t dev .devcontainer
```

* To run:
```
docker run -it -v /var/run/docker.sock:/var/run/docker.sock -v ${PWD}:/workspaces/app mperel/dev-container
```

# Editor
* vim
    * To open a file tree: `CTRL+g`
    * To goto definition: `,+g`
    * To goto docs: `,+d`
    * To cycle forward through autocomplete suggestions: `TAB`
    * To cycle backward through autocomplete suggestions: `SHIFT+TAB`
* vscode
    * Install the [Remote-Container Extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)
    * Copy the .devcontainer directory into your project
    * In the command palette, type: `Remote-Containers: Reopen in Container`
    * In the command palette, type: `Go: Install/Update Tools` and select all
    * When all tools are finished installing, in the command palette
    type: `Developer: Reload Window`
    * The docker daemon is mapped from the host into the dev container,
    so you can use docker and docker-compose commands from within
    the container as if they were run on the host