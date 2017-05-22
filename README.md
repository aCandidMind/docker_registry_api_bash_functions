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

3. By default the **Docker Registry API Bash Functions** use the following 2 API endpoints which point to the Docker Hub Registry.

      * API Authentication Endpoint: **https://auth.docker.io**
      * API Query Endpoint: **https://registry-1.docker.io**

    They can be overridden to use a private Docker Registry by setting the 2 Bash environment variables below:

      ```bash
      export DOCKER_REGISTRY_API_AUTH_ENDPOINT="https://my_docker_registry_api_authentication_endpoint"
      export DOCKER_REGISTRY_API_QUERY_ENDPOINT="https://my_docker_registry_api_query_endpoint"
      ```

3. Source the **Docker Registry API Bash Functions** file **docker_registry_api_bash_functions**. Assuming that the file **docker_registry_api_bash_functions** is in the current directory:

    ```bash
    source ./docker_registry_api_bash_functions
    ```

## Available Functions

1. **get_docker_registry_api_token** - authenticates with the Docker Hub Registry using your Docker Hub Registry credentials. This is called by the other functions below.
2. **get_docker_repo_tags** - retrieves all Tags for a Docker Repository.

   For more details on Tags refer to <https://docs.docker.com/engine/reference/commandline/image_tag/>.

3. **get_docker_image_digest** - retrieves the *Docker Content Digest* for a Docker Image.

   For more details on the *Docker Content Digest* refer to <https://docs.docker.com/registry/spec/api/#content-digests>.

4. **get_docker_image_manifest** - retrieves the Manifest for a Docker Image.

   For more details on Manifests refer to <https://docs.docker.com/registry/spec/api/#manifest>.

5. **get_docker_image_config_blob** - retrieves the configuration Blob for a Docker Image.

   For more details on Blobs refer to <https://docs.docker.com/registry/spec/api/#blob>.

## Examples

### To retrieve the Digest for the Docker Official Image "ubuntu:latest":

Note: You must prefix the Docker Image with **library/** for querying information on Docker Official Images.

#### Example:

🐳  gforghetti:~:$ **`get_docker_image_digest library/ubuntu:latest`**

#### Output:

```
{
	"DockerContentDigest": "sha256:382452f82a8bbd34443b2c727650af46aced0f94a44463c62a9848133ecb1aa8"
}
```

### To retrieve just the Digest value for the Docker Official Image "ubuntu:latest":

#### Example:

🐳  gforghetti:~:$ **`get_docker_image_digest library/ubuntu:latest | jq -r '.DockerContentDigest'`**

#### Output:

```
sha256:382452f82a8bbd34443b2c727650af46aced0f94a44463c62a9848133ecb1aa8
```

### To retrieve just the Digest value for my user "gforghetti" Docker Image "apache:latest":

Note: You must prefix the Docker Image with **___username___/** for querying information on user Docker Images.

#### Example:

🐳  gforghetti:~:$ **`get_docker_image_digest gforghetti/apache:latest | jq -r '.DockerContentDigest'`**

#### Output:

```
sha256:e18f96591975794252ed09c90bda183ff735f6a226d649099fec347cafbf5770
```

### To retrieve the tags for the Docker Official "httpd" repository:

#### Example:

🐳  gforghetti:~:$ **`get_docker_repo_tags library/httpd | jq -r '.tags[]'`**

#### Output:

```
2-alpine
2.2-alpine
2.2.29
2.2.31-alpine
2.2.31
2.2.32-alpine
2.2.32
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
2.4
2
alpine
latest
```

### To retrieve the Image Manifest for the Docker Official "mysql:5.7" image:

#### Example:

🐳  gforghetti:~:$ **`get_docker_image_manifest library/mysql:5.7 | jq`**

#### Output:

```
{
  "schemaVersion": 2,
  "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
  "config": {
    "mediaType": "application/vnd.docker.container.image.v1+json",
    "size": 6670,
    "digest": "sha256:e799c7f9ae9cd01c2e78e26952767b96aef59a5e62f02305610448da47f8ca6b"
  },
  "layers": [
    {
      "mediaType": "application/vnd.docker.image.rootfs.diff.tar.gzip",
      "size": 52584016,
      "digest": "sha256:10a267c67f423630f3afe5e04bbbc93d578861ddcc54283526222f3ad5e895b9"
    },
    {
      "mediaType": "application/vnd.docker.image.rootfs.diff.tar.gzip",
      "size": 2066,
      "digest": "sha256:c2dcc7bb2a88d3f5f2e2f57c03798de132417029412461fa5c2a3980a432ac00"
    },
    {
      "mediaType": "application/vnd.docker.image.rootfs.diff.tar.gzip",
      "size": 1307767,
      "digest": "sha256:17e7a0445698ef7ccb14c188ceef9c98785c4adf909704a86a019721e94a8eff"
    },
    {
      "mediaType": "application/vnd.docker.image.rootfs.diff.tar.gzip",
      "size": 115,
      "digest": "sha256:9a61839a176f4d61f2b383652722e0dc8f63ffd12828405b1d7d0facc903b4df"
    },
    {
      "mediaType": "application/vnd.docker.image.rootfs.diff.tar.gzip",
      "size": 10715803,
      "digest": "sha256:a1033d2f1825e47d2929ad0e15207097a5600406479ca7a67b56f50dd707fcc6"
    },
    {
      "mediaType": "application/vnd.docker.image.rootfs.diff.tar.gzip",
      "size": 19200,
      "digest": "sha256:0d6792140dcc88fa160ad0e34da08da30fdcb6e02fefd1cd071f584b5338586a"
    },
    {
      "mediaType": "application/vnd.docker.image.rootfs.diff.tar.gzip",
      "size": 217,
      "digest": "sha256:cd3adf03d6e64c0d5e59171e65490e6cb3fd552e9fd4ce81a1976ebb25447325"
    },
    {
      "mediaType": "application/vnd.docker.image.rootfs.diff.tar.gzip",
      "size": 78961159,
      "digest": "sha256:d79d216fd92b726ec5a2351140f53829dce76749cd54a92e32b520cef90a90e1"
    },
    {
      "mediaType": "application/vnd.docker.image.rootfs.diff.tar.gzip",
      "size": 936,
      "digest": "sha256:b3c25bdeb4f448f9c865abba2bb104137eba16bbe87e8507b721e01a6c27797e"
    },
    {
      "mediaType": "application/vnd.docker.image.rootfs.diff.tar.gzip",
      "size": 2732,
      "digest": "sha256:02556e8f331fdf559a93c845dc80cffad8b7c4f940ab239529756903ad07e705"
    },
    {
      "mediaType": "application/vnd.docker.image.rootfs.diff.tar.gzip",
      "size": 120,
      "digest": "sha256:4bed508a9e77fec63ff551f68e66836cbcd39404565ca36003bf54be2e533c9a"
    }
  ]
}
```

### To retrieve the Digest for the 1st Layer from the Image Manifest for the Docker Official "mysql:5.7" image:

#### Example:

🐳  gforghetti:~:$ **`get_docker_image_manifest library/mysql:5.7 | jq -r '.layers[0].digest'`**

#### Output:

```
sha256:10a267c67f423630f3afe5e04bbbc93d578861ddcc54283526222f3ad5e895b9
```

### To retrieve the Digest and Size of the 1st Layer from the Image Manifest for the Docker Official "mysql:5.7" image:

#### Example:

🐳  gforghetti:~:$ **`get_docker_image_manifest library/mysql:5.7 | jq -r '.layers[0] | .digest, .size'`**

#### Output:

```
sha256:10a267c67f423630f3afe5e04bbbc93d578861ddcc54283526222f3ad5e895b9
52584016
```

### To retrieve the configuration Blob for the Docker Official "redis:latest" image:

#### Example:

🐳  gforghetti:~:$ **`get_docker_image_config_blob library/redis:latest | jq`**

#### Output:

```
{
  "architecture": "amd64",
  "config": {
    "Hostname": "200591939db7",
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
      "GOSU_VERSION=1.7",
      "REDIS_VERSION=3.2.8",
      "REDIS_DOWNLOAD_URL=http://download.redis.io/releases/redis-3.2.8.tar.gz",
      "REDIS_DOWNLOAD_SHA1=6780d1abb66f33a97aad0edbe020403d0a15b67f"
    ],
    "Cmd": [
      "redis-server"
    ],
    "ArgsEscaped": true,
    "Image": "sha256:99d7fc950621496c5f8a6a7e22d6afff8387fe82af062ad8bb6fc70232246b27",
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
  "container": "862d805d9ac6025934a1435b6d618f12f4d6e99af41dc80b85aae1a7d3f9a672",
  "container_config": {
    "Hostname": "200591939db7",
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
      "GOSU_VERSION=1.7",
      "REDIS_VERSION=3.2.8",
      "REDIS_DOWNLOAD_URL=http://download.redis.io/releases/redis-3.2.8.tar.gz",
      "REDIS_DOWNLOAD_SHA1=6780d1abb66f33a97aad0edbe020403d0a15b67f"
    ],
    "Cmd": [
      "/bin/sh",
      "-c",
      "#(nop) ",
      "CMD [\"redis-server\"]"
    ],
    "ArgsEscaped": true,
    "Image": "sha256:99d7fc950621496c5f8a6a7e22d6afff8387fe82af062ad8bb6fc70232246b27",
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
  "created": "2017-05-10T15:43:38.559125657Z",
  "docker_version": "17.04.0-ce",
  "history": [
    {
      "created": "2017-05-08T23:28:14.437236885Z",
      "created_by": "/bin/sh -c #(nop) ADD file:f4e6551ac34ab446a297849489a5693d67a7e76c9cb9ed9346d82392c9d9a5fe in / "
    },
    {
      "created": "2017-05-08T23:28:15.327579341Z",
      "created_by": "/bin/sh -c #(nop)  CMD [\"/bin/bash\"]",
      "empty_layer": true
    },
    {
      "created": "2017-05-10T15:42:16.403369246Z",
      "created_by": "/bin/sh -c groupadd -r redis && useradd -r -g redis redis"
    },
    {
      "created": "2017-05-10T15:42:31.545491902Z",
      "created_by": "/bin/sh -c apt-get update && apt-get install -y --no-install-recommends \t\tca-certificates \t\twget \t&& rm -rf /var/lib/apt/lists/*"
    },
    {
      "created": "2017-05-10T15:42:32.376637929Z",
      "created_by": "/bin/sh -c #(nop)  ENV GOSU_VERSION=1.7",
      "empty_layer": true
    },
    {
      "created": "2017-05-10T15:42:37.151391061Z",
      "created_by": "/bin/sh -c set -x \t&& wget -O /usr/local/bin/gosu \"https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-$(dpkg --print-architecture)\" \t&& wget -O /usr/local/bin/gosu.asc \"https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-$(dpkg --print-architecture).asc\" \t&& export GNUPGHOME=\"$(mktemp -d)\" \t&& gpg --keyserver ha.pool.sks-keyservers.net --recv-keys B42F6819007F00F88E364FD4036A9C25BF357DD4 \t&& gpg --batch --verify /usr/local/bin/gosu.asc /usr/local/bin/gosu \t&& rm -r \"$GNUPGHOME\" /usr/local/bin/gosu.asc \t&& chmod +x /usr/local/bin/gosu \t&& gosu nobody true"
    },
    {
      "created": "2017-05-10T15:42:37.975083523Z",
      "created_by": "/bin/sh -c #(nop)  ENV REDIS_VERSION=3.2.8",
      "empty_layer": true
    },
    {
      "created": "2017-05-10T15:42:38.697484548Z",
      "created_by": "/bin/sh -c #(nop)  ENV REDIS_DOWNLOAD_URL=http://download.redis.io/releases/redis-3.2.8.tar.gz",
      "empty_layer": true
    },
    {
      "created": "2017-05-10T15:42:39.415038906Z",
      "created_by": "/bin/sh -c #(nop)  ENV REDIS_DOWNLOAD_SHA1=6780d1abb66f33a97aad0edbe020403d0a15b67f",
      "empty_layer": true
    },
    {
      "created": "2017-05-10T15:43:32.18792569Z",
      "created_by": "/bin/sh -c set -ex \t\t&& buildDeps=' \t\tgcc \t\tlibc6-dev \t\tmake \t' \t&& apt-get update \t&& apt-get install -y $buildDeps --no-install-recommends \t&& rm -rf /var/lib/apt/lists/* \t\t&& wget -O redis.tar.gz \"$REDIS_DOWNLOAD_URL\" \t&& echo \"$REDIS_DOWNLOAD_SHA1 *redis.tar.gz\" | sha1sum -c - \t&& mkdir -p /usr/src/redis \t&& tar -xzf redis.tar.gz -C /usr/src/redis --strip-components=1 \t&& rm redis.tar.gz \t\t&& grep -q '^#define CONFIG_DEFAULT_PROTECTED_MODE 1$' /usr/src/redis/src/server.h \t&& sed -ri 's!^(#define CONFIG_DEFAULT_PROTECTED_MODE) 1$!\\1 0!' /usr/src/redis/src/server.h \t&& grep -q '^#define CONFIG_DEFAULT_PROTECTED_MODE 0$' /usr/src/redis/src/server.h \t\t&& make -C /usr/src/redis \t&& make -C /usr/src/redis install \t\t&& rm -r /usr/src/redis \t\t&& apt-get purge -y --auto-remove $buildDeps"
    },
    {
      "created": "2017-05-10T15:43:33.721042803Z",
      "created_by": "/bin/sh -c mkdir /data && chown redis:redis /data"
    },
    {
      "created": "2017-05-10T15:43:34.435756465Z",
      "created_by": "/bin/sh -c #(nop)  VOLUME [/data]",
      "empty_layer": true
    },
    {
      "created": "2017-05-10T15:43:35.287853953Z",
      "created_by": "/bin/sh -c #(nop) WORKDIR /data",
      "empty_layer": true
    },
    {
      "created": "2017-05-10T15:43:36.346854799Z",
      "created_by": "/bin/sh -c #(nop) COPY file:9c29fbe8374a97f9c2d953c9c8b7224554607eeb7a610a930844f2bec678265c in /usr/local/bin/ "
    },
    {
      "created": "2017-05-10T15:43:37.06416255Z",
      "created_by": "/bin/sh -c #(nop)  ENTRYPOINT [\"docker-entrypoint.sh\"]",
      "empty_layer": true
    },
    {
      "created": "2017-05-10T15:43:37.807542548Z",
      "created_by": "/bin/sh -c #(nop)  EXPOSE 6379/tcp",
      "empty_layer": true
    },
    {
      "created": "2017-05-10T15:43:38.559125657Z",
      "created_by": "/bin/sh -c #(nop)  CMD [\"redis-server\"]",
      "empty_layer": true
    }
  ],
  "os": "linux",
  "rootfs": {
    "type": "layers",
    "diff_ids": [
      "sha256:8d4d1ab5ff74fc361fb74212fff3b6dc1e6c16d1e1f0e8b44f9a9112b00b564f",
      "sha256:5b9af11eed1ae93f1bb7bd3ef234a0508e3e77a1d3d50c04fe3d184b0cee7f0d",
      "sha256:88125233cfafcc3a5b973b40d219e65a69817bef40431c5c7f6b7b5cb214fe22",
      "sha256:ce9594cd6e29cce0f1ae8c5f473da7062bd083fc3542ba02575c040c129c5a8f",
      "sha256:8a985d9e7147a7b71337956ca8f69eff851cada70c1f3b41a45c8402600dc2f8",
      "sha256:4897166d3cbdd58ede3a1cadd4335ca48a84137148123a33c23b0fe4856d5903",
      "sha256:817c9875367adc7ff291650b10ffa07a67bd84bedd8a6acf028667719e28198c"
    ]
  }
}
```

### To retrieve the "Cmd" from the configuration Blob for the Docker Official "redis:latest" image:

#### Example:

🐳  gforghetti:~:$ **`get_docker_image_config_blob library/redis:latest | jq -r '.config.Cmd[]'`**

#### Output:

```
redis-server
```

### To retrieve the "Cmd" and "Entrypoint" from the configuration Blob for the Docker Official "redis:latest" image:

#### Example:

🐳  gforghetti:~:$ **`get_docker_image_config_blob library/redis:latest | jq -r '.config | .Cmd[], .Entrypoint[]'`**

#### Output:

```
redis-server
docker-entrypoint.sh
```

### To retrieve the **History** layers which are not "empty" from the configuration Blob for the Docker Official "redis:latest" image: ###

Note: Only the first **80** characters of each layer is being displayed **.created_by[0:80]**.

#### Example:

🐳  gforghetti:~:$ **`get_docker_image_config_blob library/redis:latest | jq  -r '.history[] | select(.empty_layer != 'true' ) | .created_by[0:80]'`**

#### Output:

```
/bin/sh -c #(nop) ADD file:f4e6551ac34ab446a297849489a5693d67a7e76c9cb9ed9346d82
/bin/sh -c groupadd -r redis && useradd -r -g redis redis
/bin/sh -c apt-get update && apt-get install -y --no-install-recommends 		ca-cer
/bin/sh -c set -x 	&& wget -O /usr/local/bin/gosu "https://github.com/tianon/gos
/bin/sh -c set -ex 		&& buildDeps=' 		gcc 		libc6-dev 		make 	' 	&& apt-get upda
/bin/sh -c mkdir /data && chown redis:redis /data
/bin/sh -c #(nop) COPY file:9c29fbe8374a97f9c2d953c9c8b7224554607eeb7a610a930844
```

### To retrieve multiple attributes ("docker_version, created, architecture, and os) from the configuration Blob for my user "gforghetti" Docker Image "tomcat-wildbook:latest": ###

Note: Only the first **10** characters of the **created** attribute are being selected **.created[0:10]**.

#### Example:

🐳  gforghetti:~:$ **`get_docker_image_config_blob gforghetti/tomcat-wildbook:latest | jq -r '. | .docker_version, .created[0:10], .architecture, .os'`**

#### Output:

```
17.03.1-ce
2017-05-10
amd64
linux
```