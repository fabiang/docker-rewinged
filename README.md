# docker-rewinged

Docker Image for rewinged – a self-hosted winget package source

[![fabiang/rewinged](https://img.shields.io/docker/pulls/fabiang/rewinged.svg)](https://hub.docker.com/r/fabiang/rewinged)
[![fabiang/rewinged](https://badgen.net/github/license/fabiang/docker-rewinged)](https://github.com/fabiang/docker-rewinged)
[![Docker Image](https://github.com/fabiang/docker-rewinged/actions/workflows/docker.yml/badge.svg)](https://github.com/fabiang/docker-rewinged/actions/workflows/docker.yml)

## Available tags

* 0.3.0-alpine

## Usage

```
  docker run -it --rm \
    -v "mypackagesvolume:/srv/rewinged/packages" \
    fabiang/rewinged:0.3.0-alpine \
    -version
```

## License

[BSD 2-Clause License](LICENSE).
