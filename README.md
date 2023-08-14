# fabiang/rewinged

Docker Image for [rewinged](https://github.com/jantari/rewinged) – a self-hosted winget package source

[![fabiang/rewinged](https://img.shields.io/docker/pulls/fabiang/rewinged.svg)](https://hub.docker.com/r/fabiang/rewinged)
[![fabiang/rewinged](https://badgen.net/github/license/fabiang/docker-rewinged)](https://github.com/fabiang/docker-rewinged)
[![Docker Image](https://github.com/fabiang/docker-rewinged/actions/workflows/docker.yml/badge.svg)](https://github.com/fabiang/docker-rewinged/actions/workflows/docker.yml)

## Available tags

* 0.6.0-alpine
* 0.6.0-windowsservercore-ltsc2022
* 0.6.0-windowsservercore-ltsc2019
* 0.5.0-alpine
* 0.5.0-windowsservercore-ltsc2022
* 0.5.0-windowsservercore-ltsc2019
* 0.4.0-alpine
* 0.4.0-windowsservercore-ltsc2022
* 0.4.0-windowsservercore-ltsc2019
* 0.3.0-alpine
* 0.3.0-windowsservercore-ltsc2022
* 0.3.0-windowsservercore-ltsc2019

## Usage

```
  docker run -it --rm \
    -v "mypackagesvolume:/srv/rewinged/packages" \
    fabiang/rewinged:0.5.0-alpine
```

## License

[BSD 2-Clause License](LICENSE). Rewinged itself is released under MIT license.
