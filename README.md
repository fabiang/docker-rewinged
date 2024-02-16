# fabiang/rewinged

Docker Image for [rewinged](https://github.com/jantari/rewinged) – a self-hosted winget package source

[![fabiang/rewinged](https://img.shields.io/docker/pulls/fabiang/rewinged.svg)](https://hub.docker.com/r/fabiang/rewinged)
[![fabiang/rewinged](https://badgen.net/github/license/fabiang/docker-rewinged)](https://github.com/fabiang/docker-rewinged)
[![Docker Image](https://github.com/fabiang/docker-rewinged/actions/workflows/docker.yml/badge.svg)](https://github.com/fabiang/docker-rewinged/actions/workflows/docker.yml)

## Available tags

We have images for the following operating systems:

* Alpine
* Windows Server Core LTSC2022
* Windows Server Core LTSC2019

[Check the "tags" page on Docker Hub](https://hub.docker.com/r/fabiang/rewinged/tags?page=1&ordering=name) for latest available versions.

## Usage

```
  docker run -it --rm \
    -v "mypackagesvolume:/srv/rewinged/packages" \
    fabiang/rewinged:0.7.3-alpine
```

## License

[BSD 2-Clause License](LICENSE). Rewinged itself is released under MIT license.
