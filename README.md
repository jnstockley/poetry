# Python Poetry Docker Image
This is a Docker image for Python projects that use Poetry for dependency management.

## Supported Python Versions
We follow the same supported versions as Python supports or Poetry whichever is the higher version.
i.e. if Python supports 3.9+ and Poetry supports 3.8+, we will support 3.9+.

## Supported Architectures
- linux/amd64
- linux/arm64

## Usage
### Dockerfile
```Dockerfile
FROM jnstockley/python-poetry:<poetry-version>-python-<python-version>

...

RUN poetry check # Ensure poetry project follows poetry spec
```
* `latest` is not recommend for Dockerfiles as it can change and break your build.
* `latest` will be pinned to latest poetry version AND latest python version.
* `latest` should be used to in CLIs i.e. `docker exec -it jnstockley/poetry:latest <command>`

## Things to note
- Images are built from official Python alpine images
- Poetry is installed in `/usr/local/bin`
