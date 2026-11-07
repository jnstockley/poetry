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
FROM jnstockley/poetry:2.5.1-python3.15.2

WORKDIR /app
COPY --chown=65532:65532 . .

# Ensure the project follows the Poetry specification.
RUN ["poetry", "check"]
```

Images are published to Docker Hub as `jnstockley/poetry` and to GHCR as
`ghcr.io/jnstockley/poetry`, with the same tags:

- `<poetry-version>-python<python-version>` pins both versions, as in the example above.
- `latest-python<major.minor>` follows the latest build for a Python minor version.
- `latest-python<major.minor.patch>` follows the latest Poetry build for a Python patch version.
- `latest` follows the latest Poetry and Python versions. Prefer a versioned tag in Dockerfiles to avoid unexpected changes.

To run a command in a temporary container, use
`docker run --rm jnstockley/poetry:latest poetry --version`.

## Things to note
- Images are built from Docker Hardened Python images (`dhi.io/python`).
- Runtime images run as user `65532` and do not include a shell. Use JSON exec form for `RUN` commands; shell features such as pipes and `&&` are unavailable.
- Copy project files with `--chown=65532:65532` so Poetry can write lock files and other project output.
- Poetry is installed in `/usr/local/bin`
