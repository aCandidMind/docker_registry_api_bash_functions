# Docker Registry API Bash Functions

## Introduction

The **Docker Registry API Bash Functions** is a set of Bash functions which provide a high level **query** interface for the Docker Registry HTTP API V2 to a Docker Registry.

For more information on the Docker Registry HTTP API V2 refer to <https://docs.docker.com/registry/spec/api/>.

## Setup

1. The **Docker Registry API Bash Functions** require the **curl** and **jq** commands.

    To install **curl** and **jq** if they are not currently installed:

    - Ubuntu

      ```bash
      sudo apt-get update -qq
      sudo apt-get install curl jq -y
      ```

    - RHEL/CentOS

      ```bash
      sudo yum makecache fast
      sudo yum install curl jq -y
      ```

    - MacOS

      If you are running MacOS you can install **curl** and **jq** with Homebrew <https://brew.sh/>.
      ```bash
      brew install curl jq
      ```

2. You also need to define your Docker Registry credentials in environment variables:

    * You must export the following Bash variables which specify your Docker Registry credentials.

      - **DOCKER_USER** - Your Docker Registry user account.
      - **DOCKER_PASSWORD** - Your Docker Registry user account password.

      Example:

      ```bash
      export DOCKER_USER="my_docker_registry_user_account"
      export DOCKER_PASSWORD="my_docker_registry_user_account_password"
      ```

3. Source the **Docker Registry API Bash Functions** file **docker_registry_api_bash_functions**. Assuming that the file **docker_registry_api_bash_functions** is in the current directory:

    ```bash
    source ./docker_registry_api_bash_functions
    ```

## Available Functions

1. **get_docker_registry_api_token** - authenticates with the Docker Registry using your Docker credentials. This is called by the other functions below.
2. **get_docker_repo_tags** - retrieves all Tags for a Docker Repository.

   For more details on Tags refer to <https://docs.docker.com/engine/reference/commandline/image_tag/>.

3. **get_docker_image_digest** - retrieves the *Docker Content Digest* for a Docker Image.

   For more details on the *Docker Content Digest* refer to <https://docs.docker.com/registry/spec/api/#content-digests>.

4. **get_docker_image_manifest** - retrieves the Manifest for a Docker Image.

   For more details on Manifests refer to <https://docs.docker.com/registry/spec/api/#manifest>.

5. **get_docker_image_config_blob** - retrieves the configuration Blob for a Docker Image.

   For more details on Blobs refer to <https://docs.docker.com/registry/spec/api/#blob>.

## Command Help

### get_docker_repo_tags

```
🐳  gforghetti:~:$ get_docker_repo_tags -h
```

```
Usage: get_docker_repo_tags -r docker-repo [-t {image|plugin}] [-k]
 -r is the Docker Repo and must be specified.
 -t {image|plugin} Default is image
 -k disable SSL verification for an insecure private Docker Trusted Registry
```

### get_docker_image_digest

```
🐳  gforghetti:~:$ get_docker_image_digest -h
```
```
Usage: get_docker_image_digest -i docker-image [-t {image|plugin}] [-k]
 -i is the Docker Image and must be specified.
 -t {image|plugin} Default is image
 -k disable SSL verification for an insecure private Docker Trusted Registry
```

### get_docker_image_manifest

```
🐳  gforghetti:~: $ get_docker_image_manifest -h
```

```
Usage: get_docker_image_manifest -i docker-image [-t {image|plugin}] [-k]
 -i is the Docker Image and must be specified.
 -t {image|plugin} Default is image
 -k disable SSL verification for an insecure private Docker Trusted Registry
 ```

### get_docker_image_manifests (Returns the Fat Manifest - Multi Architecture Support)

```
🐳  gforghetti:~: $ get_docker_image_manifests -h
```

```
Usage: get_docker_image_manifests -i docker-image [-t {image|plugin}] [-k]
 -i is the Docker Image and must be specified.
 -t {image|plugin} Default is image
 -k disable SSL verification for an insecure private Docker Trusted Registry
 ```

### get_docker_image_config_blob

```
🐳  gforghetti:~: $ get_docker_image_config_blob -h
```

```
Usage: get_docker_image_config_blob -i docker-image [-t {image|plugin}] [-k]
 -i is the Docker Image and must be specified.
 -t {image|plugin} Default is image
 -k disable SSL verification for an insecure private Docker Trusted Registry
```

## Examples

### To retrieve the Digest for the Docker Official Image "ubuntu:latest":

Note: You must prefix the Docker Image with **library/** for querying information on Docker Official Images.

#### Example:

```
🐳  gforghetti:~:$ get_docker_image_digest -i library/ubuntu:latest
```

#### Output:

```
{
   "DockerContentDigest": "sha256:5f4bdc3467537cbbe563e80db2c3ec95d548a9145d64453b06939c4592d67b6d"
}
```

### To retrieve just the Digest value for my user "gforghetti" Docker Image "apache:latest":

Note: You must prefix the Docker Image with **___username___/** for querying information on user Docker Images.

#### Example:

```
🐳  gforghetti:~:$ get_docker_image_digest -i gforghetti/apache:latest | jq -r '.DockerContentDigest'
```

#### Output:

```
sha256:e18f96591975794252ed09c90bda183ff735f6a226d649099fec347cafbf5770
```

### To retrieve just the Digest for the Docker Image "172.16.129.76/gforghetti/tomcat-wildbook:latest" which is in an insecure private Docker Trusted Registry:

#### Example:

```
🐳  gforghetti:~:$ get_docker_image_digest -i 172.16.129.76/gforghetti/tomcat-wildbook:latest -k | jq -r '.DockerContentDigest'
```

#### Output:

```
sha256:66a82c818640de6dc42c3bcc786b7b3c8e669168d0b65b65126681948e7636e4"
```

### To retrieve the tags for the Docker Official "httpd" repository:

#### Example:

```
🐳  gforghetti:~:$ get_docker_repo_tags -r library/httpd | jq -r '.tags[]'
```

#### Output:

```
2-alpine
2.2-alpine
2.2.29
2.2.31-alpine
2.2.31
2.2.32-alpine
2.2.32
2.2.34-alpine
2.2.34
2.2
2.4-alpine
2.4.10
2.4.12
2.4.16
2.4.17
2.4.18
2.4.20
2.4.23-alpine
2.4.23
2.4.25-alpine
2.4.25
2.4.27-alpine
2.4.27
2.4.28-alpine
2.4.28
2.4.29-alpine
2.4.29
2.4.32-alpine
2.4.32
2.4.33-alpine
2.4.33
2.4
2
alpine
latest
```

#### To retrieve the tags for the Docker Image "172.16.129.76/gforghetti/tomcat-wildbook:latest" which is in an insecure private Docker Trusted Registry:

```
🐳  gforghetti:~:$ get_docker_repo_tags -r 172.16.129.76/gforghetti/tomcat-wildbook -k | jq
```

```
{
  "name": "gforghetti/tomcat-wildbook",
  "tags": [
    "latest"
  ]
}
```

### To retrieve the Image Manifest for the Docker Official "mysql:5.7" image:

#### Example:

```
🐳  gforghetti:~:$ get_docker_image_manifest -i library/mysql:5.7 | jq
```

#### Output:

```
{
  "schemaVersion": 2,
  "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
  "config": {
    "mediaType": "application/vnd.docker.container.image.v1+json",
    "size": 6869,
    "digest": "sha256:66bc0f66b7af6ba3ea96582685d3afcd6dff93c2f8999da0ffadd67b280db548"
  },
  "layers": [
    {
      "mediaType": "application/vnd.docker.image.rootfs.diff.tar.gzip",
      "size": 22496975,
      "digest": "sha256:683abbb4ea60e108164f1d351e7bcf13daf45941137d800086447874df05f48e"
    },
    {
      "mediaType": "application/vnd.docker.image.rootfs.diff.tar.gzip",
      "size": 1747,
      "digest": "sha256:0550d17aeefaf608f887e92fa2ccb50b43e4f7b65d354d61c97217aa4a4cab80"
    },
    {
      "mediaType": "application/vnd.docker.image.rootfs.diff.tar.gzip",
      "size": 4498578,
      "digest": "sha256:7e26605ddd77b819dfb23d28e1992d8f435142b1b70c8aceed3e94de1c69ca46"
    },
    {
      "mediaType": "application/vnd.docker.image.rootfs.diff.tar.gzip",
      "size": 1270486,
      "digest": "sha256:9882737bd15fd5c289096db0652851ae9b0aba5ea95758b091fb35dc3544b151"
    },
    {
      "mediaType": "application/vnd.docker.image.rootfs.diff.tar.gzip",
      "size": 115,
      "digest": "sha256:999c06ab75f6240facacea7f7bef87355c1b3b0b2c99df58c165d30a9703f57e"
    },
    {
      "mediaType": "application/vnd.docker.image.rootfs.diff.tar.gzip",
      "size": 12091205,
      "digest": "sha256:c71d695f9937f6f3880061b5e3d2963fc9ae02ea92e1cba8e168292940f79ba3"
    },
    {
      "mediaType": "application/vnd.docker.image.rootfs.diff.tar.gzip",
      "size": 22327,
      "digest": "sha256:c38f847c1491e59cd3757df5afcd9deadfef755eb5ec055707912a96dafcf79f"
    },
    {
      "mediaType": "application/vnd.docker.image.rootfs.diff.tar.gzip",
      "size": 220,
      "digest": "sha256:74f9c61f40bfe8a9b0bf1195ba1ea56fa0768aec0564def3b27431e979382cf4"
    },
    {
      "mediaType": "application/vnd.docker.image.rootfs.diff.tar.gzip",
      "size": 83462574,
      "digest": "sha256:30b252a90a12371e016b860e205611d66ed65bc0a21b06fd2e5e8841786f1920"
    },
    {
      "mediaType": "application/vnd.docker.image.rootfs.diff.tar.gzip",
      "size": 2868,
      "digest": "sha256:9f92ebb7da5511661cabe08b0a34e8eea5f223f26948af6aa904e236eb7fa465"
    },
    {
      "mediaType": "application/vnd.docker.image.rootfs.diff.tar.gzip",
      "size": 121,
      "digest": "sha256:90303981d2767b76a3d019764dbaa4f7006868073a0bb6fe4a338fa3c3afaee8"
    }
  ]
}
```

### To retrieve the Digest of the 1st layer from the Image Manifest for the Docker Official "mysql:5.7" image:

#### Example:

```
🐳  gforghetti:~:$ get_docker_image_manifest -i library/mysql:5.7 | jq -r '.layers[0].digest'
```

#### Output:

```
sha256:683abbb4ea60e108164f1d351e7bcf13daf45941137d800086447874df05f48e
```

### To retrieve the Digest and Size of the 1st Layer from the Image Manifest for the Docker Official "mysql:5.7" image:

#### Example:

```
🐳  gforghetti:~:$ get_docker_image_manifest -i library/mysql:5.7 | jq -r '.layers[0] | .digest, .size'
```

#### Output:

```
sha256:683abbb4ea60e108164f1d351e7bcf13daf45941137d800086447874df05f48e
22496975
```

### To retrieve the Fat Docker Image Manifest for the Docker Official "ubuntu:latest" image:

#### Example:

```
🐳  gforghetti:[~/Vagrants/ACME-Docker-Solution-Briefs] $ get_docker_image_manifests -i library/ubuntu:latest | jq 'if .mediaType?=="application/vnd.docker.distribution.manifest.list.v2+json" then . else error("Docker Image is Not Multi Architecture!") end'
```

#### Output:

```
{
  "schemaVersion": 2,
  "mediaType": "application/vnd.docker.distribution.manifest.list.v2+json",
  "manifests": [
    {
      "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
      "size": 1357,
      "digest": "sha256:e7def0d56013d50204d73bb588d99e0baa7d69ea1bc1157549b898eb67287612",
      "platform": {
        "architecture": "amd64",
        "os": "linux"
      }
    },
    {
      "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
      "size": 1357,
      "digest": "sha256:5b5f606437e81499737fae5e4419fe331362ea306adac1823a7eeb33f60913f2",
      "platform": {
        "architecture": "arm",
        "os": "linux",
        "variant": "v7"
      }
    },
    {
      "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
      "size": 1357,
      "digest": "sha256:1356a2700f74c312bff5edad20290e212123102a56545d2f1086e6313d0fe38e",
      "platform": {
        "architecture": "arm64",
        "os": "linux",
        "variant": "v8"
      }
    },
    {
      "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
      "size": 1357,
      "digest": "sha256:23aaa037c5d628464967fc7586a717c020cda939bccb0bf79a367356d4fbf4cd",
      "platform": {
        "architecture": "386",
        "os": "linux"
      }
    },
    {
      "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
      "size": 1357,
      "digest": "sha256:f81270f3dd7a73154db578102a0e4afa98b34f3aa009a6f4c9770874a4b34523",
      "platform": {
        "architecture": "ppc64le",
        "os": "linux"
      }
    },
    {
      "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
      "size": 1357,
      "digest": "sha256:f13c8e57117a2b1373d511c0b014899ad7401142057964fce9df6c6d224fd989",
      "platform": {
        "architecture": "s390x",
        "os": "linux"
      }
    }
  ]
}
```

### To retrieve the configuration Blob for the Docker Official "redis:latest" image:

#### Example:

```
🐳  gforghetti:~:$ get_docker_image_config_blob -i library/redis:latest | jq
```

#### Output:

```
{
  "architecture": "amd64",
  "config": {
    "Hostname": "",
    "Domainname": "",
    "User": "",
    "AttachStdin": false,
    "AttachStdout": false,
    "AttachStderr": false,
    "ExposedPorts": {
      "6379/tcp": {}
    },
    "Tty": false,
    "OpenStdin": false,
    "StdinOnce": false,
    "Env": [
      "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
      "GOSU_VERSION=1.10",
      "REDIS_VERSION=4.0.10",
      "REDIS_DOWNLOAD_URL=http://download.redis.io/releases/redis-4.0.10.tar.gz",
      "REDIS_DOWNLOAD_SHA=1db67435a704f8d18aec9b9637b373c34aa233d65b6e174bdac4c1b161f38ca4"
    ],
    "Cmd": [
      "redis-server"
    ],
    "ArgsEscaped": true,
    "Image": "sha256:d84b18dfd4e7c0574c1feba2bd8efd51a90dd7706f16f33b0bbdcc15b78d589b",
    "Volumes": {
      "/data": {}
    },
    "WorkingDir": "/data",
    "Entrypoint": [
      "docker-entrypoint.sh"
    ],
    "OnBuild": [],
    "Labels": null
  },
  "container": "fa3f101e57e36e0c8ff4e779043ac51c9aa06bf1acd8af74fadc4b55097fdd3b",
  "container_config": {
    "Hostname": "fa3f101e57e3",
    "Domainname": "",
    "User": "",
    "AttachStdin": false,
    "AttachStdout": false,
    "AttachStderr": false,
    "ExposedPorts": {
      "6379/tcp": {}
    },
    "Tty": false,
    "OpenStdin": false,
    "StdinOnce": false,
    "Env": [
      "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
      "GOSU_VERSION=1.10",
      "REDIS_VERSION=4.0.10",
      "REDIS_DOWNLOAD_URL=http://download.redis.io/releases/redis-4.0.10.tar.gz",
      "REDIS_DOWNLOAD_SHA=1db67435a704f8d18aec9b9637b373c34aa233d65b6e174bdac4c1b161f38ca4"
    ],
    "Cmd": [
      "/bin/sh",
      "-c",
      "#(nop) ",
      "CMD [\"redis-server\"]"
    ],
    "ArgsEscaped": true,
    "Image": "sha256:d84b18dfd4e7c0574c1feba2bd8efd51a90dd7706f16f33b0bbdcc15b78d589b",
    "Volumes": {
      "/data": {}
    },
    "WorkingDir": "/data",
    "Entrypoint": [
      "docker-entrypoint.sh"
    ],
    "OnBuild": [],
    "Labels": {}
  },
  "created": "2018-06-27T01:18:26.908214534Z",
  "docker_version": "17.06.2-ce",
  "history": [
    {
      "created": "2018-06-26T21:25:25.408453148Z",
      "created_by": "/bin/sh -c #(nop) ADD file:28fbc9fd012eef72780d1c75fc2b0969d13f0138f735035335ab4b76793da2da in / "
    },
    {
      "created": "2018-06-26T21:25:25.735257578Z",
      "created_by": "/bin/sh -c #(nop)  CMD [\"bash\"]",
      "empty_layer": true
    },
    {
      "created": "2018-06-27T01:15:28.126881138Z",
      "created_by": "/bin/sh -c groupadd -r redis && useradd -r -g redis redis"
    },
    {
      "created": "2018-06-27T01:15:28.341993652Z",
      "created_by": "/bin/sh -c #(nop)  ENV GOSU_VERSION=1.10",
      "empty_layer": true
    },
    {
      "created": "2018-06-27T01:15:50.240137625Z",
      "created_by": "/bin/sh -c set -ex; \t\tfetchDeps=\" \t\tca-certificates \t\tdirmngr \t\tgnupg \t\twget \t\"; \tapt-get update; \tapt-get install -y --no-install-recommends $fetchDeps; \trm -rf /var/lib/apt/lists/*; \t\tdpkgArch=\"$(dpkg --print-architecture | awk -F- '{ print $NF }')\"; \twget -O /usr/local/bin/gosu \"https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-$dpkgArch\"; \twget -O /usr/local/bin/gosu.asc \"https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-$dpkgArch.asc\"; \texport GNUPGHOME=\"$(mktemp -d)\"; \tgpg --keyserver ha.pool.sks-keyservers.net --recv-keys B42F6819007F00F88E364FD4036A9C25BF357DD4; \tgpg --batch --verify /usr/local/bin/gosu.asc /usr/local/bin/gosu; \tgpgconf --kill all; \trm -r \"$GNUPGHOME\" /usr/local/bin/gosu.asc; \tchmod +x /usr/local/bin/gosu; \tgosu nobody true; \t\tapt-get purge -y --auto-remove $fetchDeps"
    },
    {
      "created": "2018-06-27T01:17:51.049852464Z",
      "created_by": "/bin/sh -c #(nop)  ENV REDIS_VERSION=4.0.10",
      "empty_layer": true
    },
    {
      "created": "2018-06-27T01:17:51.257582616Z",
      "created_by": "/bin/sh -c #(nop)  ENV REDIS_DOWNLOAD_URL=http://download.redis.io/releases/redis-4.0.10.tar.gz",
      "empty_layer": true
    },
    {
      "created": "2018-06-27T01:17:51.463335483Z",
      "created_by": "/bin/sh -c #(nop)  ENV REDIS_DOWNLOAD_SHA=1db67435a704f8d18aec9b9637b373c34aa233d65b6e174bdac4c1b161f38ca4",
      "empty_layer": true
    },
    {
      "created": "2018-06-27T01:18:24.449938413Z",
      "created_by": "/bin/sh -c set -ex; \t\tbuildDeps=' \t\twget \t\t\t\tgcc \t\tlibc6-dev \t\tmake \t'; \tapt-get update; \tapt-get install -y $buildDeps --no-install-recommends; \trm -rf /var/lib/apt/lists/*; \t\twget -O redis.tar.gz \"$REDIS_DOWNLOAD_URL\"; \techo \"$REDIS_DOWNLOAD_SHA *redis.tar.gz\" | sha256sum -c -; \tmkdir -p /usr/src/redis; \ttar -xzf redis.tar.gz -C /usr/src/redis --strip-components=1; \trm redis.tar.gz; \t\tgrep -q '^#define CONFIG_DEFAULT_PROTECTED_MODE 1$' /usr/src/redis/src/server.h; \tsed -ri 's!^(#define CONFIG_DEFAULT_PROTECTED_MODE) 1$!\\1 0!' /usr/src/redis/src/server.h; \tgrep -q '^#define CONFIG_DEFAULT_PROTECTED_MODE 0$' /usr/src/redis/src/server.h; \t\tmake -C /usr/src/redis -j \"$(nproc)\"; \tmake -C /usr/src/redis install; \t\trm -r /usr/src/redis; \t\tapt-get purge -y --auto-remove $buildDeps"
    },
    {
      "created": "2018-06-27T01:18:25.292917585Z",
      "created_by": "/bin/sh -c mkdir /data && chown redis:redis /data"
    },
    {
      "created": "2018-06-27T01:18:25.532325332Z",
      "created_by": "/bin/sh -c #(nop)  VOLUME [/data]",
      "empty_layer": true
    },
    {
      "created": "2018-06-27T01:18:25.804741333Z",
      "created_by": "/bin/sh -c #(nop) WORKDIR /data",
      "empty_layer": true
    },
    {
      "created": "2018-06-27T01:18:26.205704849Z",
      "created_by": "/bin/sh -c #(nop) COPY file:9c29fbe8374a97f9c2d953c9c8b7224554607eeb7a610a930844f2bec678265c in /usr/local/bin/ "
    },
    {
      "created": "2018-06-27T01:18:26.46062041Z",
      "created_by": "/bin/sh -c #(nop)  ENTRYPOINT [\"docker-entrypoint.sh\"]",
      "empty_layer": true
    },
    {
      "created": "2018-06-27T01:18:26.69209863Z",
      "created_by": "/bin/sh -c #(nop)  EXPOSE 6379/tcp",
      "empty_layer": true
    },
    {
      "created": "2018-06-27T01:18:26.908214534Z",
      "created_by": "/bin/sh -c #(nop)  CMD [\"redis-server\"]",
      "empty_layer": true
    }
  ],
  "os": "linux",
  "rootfs": {
    "type": "layers",
    "diff_ids": [
      "sha256:9c46f426bcb704beffafc951290ee7fe05efddbc7406500e7d0a3785538b8735",
      "sha256:5d91794593bba502923c05812ca7bebf614aafff91343af5e6bb48e35e22806e",
      "sha256:e3192998e317bb353116cc4598fc185e6ae32529cb6a25e5f3a3745b57e76d81",
      "sha256:45648ff3360d4ca2d10abb663ef56b683812add3cddc51b90f17fb52c1aba128",
      "sha256:6c0bd81580c995445802cab06b6f6be6203ab34f1a2e61c41cd1eab5b24bc25f",
      "sha256:911e1ec2b8b86cb23f80facd7153051afcbacf24fa0fd51ee0c644f8fc8ba649"
    ]
  }
}
```

### To retrieve the "Cmd" from the configuration Blob for the Docker Official "redis:latest" image:

#### Example:

```
🐳  gforghetti:~:$ get_docker_image_config_blob -i library/redis:latest | jq -r '.config.Cmd[]'
```

#### Output:

```
redis-server
```

### To retrieve the "Cmd" and "Entrypoint" from the configuration Blob for the Docker Official "redis:latest" image:

#### Example:

```
🐳  gforghetti:~:$ get_docker_image_config_blob -i library/redis:latest | jq -r '.config | .Cmd[], .Entrypoint[]'
```

#### Output:

```
redis-server
docker-entrypoint.sh
```

### To retrieve the **History** layers which are not "empty" from the configuration Blob for the Docker Official "redis:latest" image: ###

Note: Only the first **80** characters of each layer is being displayed **.created_by[0:80]**.

#### Example:

```
🐳  gforghetti:~:$ get_docker_image_config_blob -i library/redis:latest | jq  -r '.history[] | select(.empty_layer != 'true' ) | .created_by[0:80]'
```

#### Output:

```
/bin/sh -c #(nop) ADD file:28fbc9fd012eef72780d1c75fc2b0969d13f0138f735035335ab4
/bin/sh -c groupadd -r redis && useradd -r -g redis redis
/bin/sh -c set -ex; 		fetchDeps=" 		ca-certificates 		dirmngr 		gnupg 		wget 	";
/bin/sh -c set -ex; 		buildDeps=' 		wget 				gcc 		libc6-dev 		make 	'; 	apt-get
/bin/sh -c mkdir /data && chown redis:redis /data
/bin/sh -c #(nop) COPY file:9c29fbe8374a97f9c2d953c9c8b7224554607eeb7a610a930844
```

### To retrieve multiple attributes ("docker_version, created, architecture, and os) from the configuration Blob for my user "gforghetti" Docker Image "tomcat-wildbook:latest": ###

Note: Only the first **10** characters of the **created** attribute are being selected **.created[0:10]**.

#### Example:

```
🐳  gforghetti:~:$ get_docker_image_config_blob -i gforghetti/tomcat-wildbook:latest | jq -r '. | .docker_version, .created[0:10], .architecture, .os'
```

#### Output:

```
18.05.0-ce
2018-05-27
amd64
linux
```