# docker-guide

```json
{
  "insecure-registries": [
    "182.92.214.141:8082",
  ],
  "registry-mirrors":[
    "https://docker.m.daocloud.io"
  ],
  "data-root": "/path/to/your/new/docker/root"
  "max-concurrent-downloads": 20,
  "max-concurrent-uploads": 20
}
```

See also https://github.com/LinuxSuRen/container-assistant

## As Proxy
We could start an registry server as any kind of container registry, such as: ghcr.io

First, get the default config of registry:
```shell
docker run registry cat /etc/docker/registry/config.yml > config.yml
```

then, put the following part into it:
```yaml
proxy:
  remoteurl: https://ghcr.io
```

then, start the server as the following command:
```shell
docker run -v /root/.config/docker:/etc/docker/registry \
  -v /var/lib/registry:/var/lib/registry --restart always -d \
  --network host registry
```

finally, you use use this mirror after updating the Docker daemon:
```json
# find file from /etc/docker/daemon.json
{
  "registry-mirrors":["http://10.121.218.184:5000"],
  "insecure-registries": ["http://10.121.218.184:5000"]
}
```

See also the following references:

* https://docs.docker.com/registry/recipes/mirror/
* https://github.com/distribution/distribution
