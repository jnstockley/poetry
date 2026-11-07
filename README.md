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
- `<poetry-major>-python-<python-version>` follows Poetry releases within that major version, e.g. `2-python-3.15`.
- `<poetry-major.minor>-python-<python-version>` follows Poetry patch releases within that minor version, e.g. `2.5-python-3.15`.
- `<poetry-major.minor.patch>-python-<python-version>` selects a specific Poetry release, e.g. `2.5.1-python-3.15`.
- `latest-python<major.minor>` follows the latest build for a Python minor version.
- `latest-python<major.minor.patch>` follows the latest Poetry build for a Python patch version.
- `latest` follows the latest Poetry and Python versions. Prefer a versioned tag in Dockerfiles to avoid unexpected changes.

The Poetry version aliases are derived automatically from the installed Poetry version
on each build. Each alias is available with both the Python minor version (e.g. `3.15`)
and full version (e.g. `3.15.2`). Pull request builds use the same aliases with a
`testing-` prefix.

To run a command in a temporary container, use
`docker run --rm jnstockley/poetry:latest poetry --version`.

## Things to note
- Images are built from Docker Hardened Python images (`dhi.io/python`).
- Runtime images run as user `65532` and do not include a shell. Use JSON exec form for `RUN` commands; shell features such as pipes and `&&` are unavailable.
- Copy project files with `--chown=65532:65532` so Poetry can write lock files and other project output.
- Poetry is installed in `/usr/local/bin`
