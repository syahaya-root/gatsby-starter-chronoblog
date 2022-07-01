---
title: Ruby On Rails
cover: ./dockerruby.png
date: 2022-07-01
description: Gitlab-Runner
tags: ['Git']
---

## Installation Gitlab-Runner

- Download repository Gitlab:
```
curl -L "https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh" | sudo bash
```
- Install gitlab-runner latest or you can install specific version:
```
sudo apt-get install gitlab-runner
```
- For specific version:
```
apt-cache madison gitlab-runner (Check Version Available)
apt-get install gitlab-runner=<VERSION>
```
- Check version:
```
gitlab-runner --version
```

## Register a Runner

- Run command
```
sudo gitlab-runner register
```
- Input GitLab instance URL
- Input Token, you can get token in the setting => ci/cd => runners
- Description for the runner
- Input a tag for runner, it can be changed later in Gitlab UI
- Maintenance note for runner
- Choice the runner executor, usually "docker"


## Configuration

If you configure runner in "root", its the path:
```
/etc/gitlab-runner/config.toml 
```
For "non-root":
```
.gitlab-runner/config.toml
```
- You can follow the example template:
```
concurrent = 1
check_interval = 0

[session_server]
  session_timeout = 1800
  
[[runners]]
  name = "runner"
  url = "https://gitlab.com/"
  token = "rwsoxxxxxxx"
  executor = "docker"
  [runners.custom_build_dir]
  [runners.cache]
    [runners.cache.s3]
    [runners.cache.gcs]
    [runners.cache.azure]
  [runners.docker]
    tls_verify = false
    image = "docker:stable"
    privileged = false
    disable_entrypoint_overwrite = false
    oom_kill_disable = false
    disable_cache = false
    volumes = ["/var/run/docker.sock:/var/run/docker.sock", "/cache"]
    shm_size = 0
```
