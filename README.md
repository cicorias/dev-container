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
docker build -t mperel/dev-container .
```

* To run:
```
docker run -it -v /var/run/docker.sock:/var/run/docker.sock -v ${PWD}:/workspaces/app mperel/dev-container
```

# Editor
* To open a file tree: `CTRL+g`
* To goto definition: `,+g`
* To goto docs: `,+d`
* To cycle forward through autocomplete suggestions: `TAB`
* To cycle backward through autocomplete suggestions: `SHIFT+TAB`