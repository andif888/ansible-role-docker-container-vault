# ansible-role-docker-container-vault

Role to run Vault in a docker container

## Table of content

- [Default Variables](#default-variables)
  - [docker_container_vault_auto_unseal](#docker_container_vault_auto_unseal)
  - [docker_container_vault_capabilities](#docker_container_vault_capabilities)
  - [docker_container_vault_command](#docker_container_vault_command)
  - [docker_container_vault_env](#docker_container_vault_env)
  - [docker_container_vault_image](#docker_container_vault_image)
  - [docker_container_vault_labels](#docker_container_vault_labels)
  - [docker_container_vault_name](#docker_container_vault_name)
  - [docker_container_vault_networks](#docker_container_vault_networks)
  - [docker_container_vault_ports](#docker_container_vault_ports)
  - [docker_container_vault_restic_enable](#docker_container_vault_restic_enable)
  - [docker_container_vault_restic_retention](#docker_container_vault_restic_retention)
  - [docker_container_vault_restic_s3_bucket_name](#docker_container_vault_restic_s3_bucket_name)
  - [docker_container_vault_restic_s3_endpoint](#docker_container_vault_restic_s3_endpoint)
  - [docker_container_vault_restic_s3_repo](#docker_container_vault_restic_s3_repo)
  - [docker_container_vault_restic_s3_repo_access_key](#docker_container_vault_restic_s3_repo_access_key)
  - [docker_container_vault_restic_s3_repo_password](#docker_container_vault_restic_s3_repo_password)
  - [docker_container_vault_restic_s3_repo_secret_key](#docker_container_vault_restic_s3_repo_secret_key)
  - [docker_container_vault_restic_tag](#docker_container_vault_restic_tag)
  - [docker_container_vault_volume_dir](#docker_container_vault_volume_dir)
  - [docker_container_vault_volumes](#docker_container_vault_volumes)
  - [docker_image_vault_name](#docker_image_vault_name)
  - [docker_image_vault_pull](#docker_image_vault_pull)
  - [docker_network_vault_name](#docker_network_vault_name)
- [Discovered Tags](#discovered-tags)
- [Dependencies](#dependencies)
- [License](#license)
- [Author](#author)

---

## Default Variables

### docker_container_vault_auto_unseal

Set to true to auto unseal

#### Default value

```YAML
docker_container_vault_auto_unseal: false
```

### docker_container_vault_capabilities

List of docker capabilities

#### Default value

```YAML
docker_container_vault_capabilities:
  - IPC_LOCK
```

### docker_container_vault_command

docker command

#### Default value

```YAML
docker_container_vault_command: server
```

### docker_container_vault_env

Dictionery of key,value pairs for docker environment variables to configure vault.

#### Default value

```YAML
docker_container_vault_env:
  VAULT_ADDR: http://127.0.0.1:8200
  VAULT_LOCAL_CONFIG: '{"backend": {"file": {"path": "/vault/data"}}, "listener":
    {"tcp": {"address": "0.0.0.0:8200", "tls_disable": "true"}}, "default_lease_ttl":
    "168h", "max_lease_ttl": "720h", "ui": "true"}'
```

### docker_container_vault_image

Repository path and tag used to create the container. If an image is not found or pull is true, the image will be pulled from the registry. If no tag is included, latest will be used.

#### Default value

```YAML
docker_container_vault_image: '{{ docker_image_vault_name }}'
```

### docker_container_vault_labels

Dictionary of key value pairs for container labels.

Example:

```yaml

docker_container_vault_labels:

traefik.enable: "true"

```

#### Default value

```YAML
docker_container_vault_labels: {}
```

### docker_container_vault_name

Name for the container

#### Default value

```YAML
docker_container_vault_name: vault
```

### docker_container_vault_networks

List of networks the container belongs to.

#### Default value

```YAML
docker_container_vault_networks:
  - name: '{{ docker_network_vault_name }}'
```

### docker_container_vault_ports

List of ports to publish from the container to the host.

#### Default value

```YAML
docker_container_vault_ports:
  - 8200:8200
```

### docker_container_vault_restic_enable

Enable restic backup for the container's mounted volumes.

#### Default value

```YAML
docker_container_vault_restic_enable: false
```

### docker_container_vault_restic_retention

Retention settions for `restic forget` after the `restic backup`.

#### Default value

```YAML
docker_container_vault_restic_retention:
  keep_last: 1
  keep_daily: 7
  keep_weekly: 4
```

### docker_container_vault_restic_s3_bucket_name

Minio S3 bucket name for restic backup storage.

#### Default value

```YAML
docker_container_vault_restic_s3_bucket_name: restic-{{ docker_container_vault_name
  }}
```

### docker_container_vault_restic_s3_endpoint

Minio S3 endpoint for restic backup storage.

Example:

```yaml

docker_container__base__restic_s3_endpoint: "https://minio.{{ dns_domain }}"

docker_container_vault_restic_s3_endpoint: "{{ docker_container__base__restic_s3_endpoint }}"

```

#### Default value

```YAML
docker_container_vault_restic_s3_endpoint: '{{ docker_container__base__restic_s3_endpoint
  }}'
```

### docker_container_vault_restic_s3_repo

Minio S3 repo URL for restic backup storage.

#### Default value

```YAML
docker_container_vault_restic_s3_repo: s3:{{ docker_container_vault_restic_s3_endpoint
  }}/{{ docker_container_vault_restic_s3_bucket_name }}
```

### docker_container_vault_restic_s3_repo_access_key

Minio S3 repo access key for restic backup storage.

#### Default value

```YAML
docker_container_vault_restic_s3_repo_access_key: '{{ docker_container__base__restic_s3_repo_access_key
  }}'
```

### docker_container_vault_restic_s3_repo_password

Minio S3 repo password for restic backup storage.

#### Default value

```YAML
docker_container_vault_restic_s3_repo_password: '{{ docker_container__base__restic_s3_repo_password
  }}'
```

### docker_container_vault_restic_s3_repo_secret_key

Minio S3 repo secret key for restic backup storage.

#### Default value

```YAML
docker_container_vault_restic_s3_repo_secret_key: '{{ docker_container__base__restic_s3_repo_secret_key
  }}'
```

### docker_container_vault_restic_tag

Tag for the `restic backup` command

#### Default value

```YAML
docker_container_vault_restic_tag: '{{ docker_container_vault_name }}'
```

### docker_container_vault_volume_dir

Volume mount host directory, where Treafik config files are stored.

#### Default value

```YAML
docker_container_vault_volume_dir: '{{ docker_container__base__volume_dir }}/{{ docker_container_vault_name
  }}'
```

### docker_container_vault_volumes

List of volumes to mount within the container.

#### Default value

```YAML
docker_container_vault_volumes:
  - '{{ docker_container_vault_volume_dir }}/config:/vault/config'
  - '{{ docker_container_vault_volume_dir }}/policies:/vault/policies'
  - '{{ docker_container_vault_volume_dir }}/data:/vault/data'
  - '{{ docker_container_vault_volume_dir }}/logs:/vault/logs'
```

### docker_image_vault_name

Repository path and tag for the container image.

#### Default value

```YAML
docker_image_vault_name: vault
```

### docker_image_vault_pull

Indicate to always pull the docker image.

#### Default value

```YAML
docker_image_vault_pull: false
```

### docker_network_vault_name

Name of the docker network created for vault.

#### Default value

```YAML
docker_network_vault_name: '{{ docker_container_vault_name }}_backend'
```

## Discovered Tags

**_docker-container-backup-all_**\
&emsp;Backup all containers' volume mounts.

**_docker-container-backup-init-all_**\
&emsp;Run init backup task for all container.

**_docker-container-backup-init-vault_**\
&emsp;Run init backup task for vault if restic is enabled.

**_docker-container-backup-list-all_**\
&emsp;List all containers' backups.

**_docker-container-backup-list-vault_**\
&emsp;List vault backups.

**_docker-container-backup-vault_**\
&emsp;Backup vault volume mounts.

**_docker-container-prereq-all_**\
&emsp;Ensure all pre-requisites are installed

**_docker-container-prereq-vault_**\
&emsp;Ensure all pre-requisites for vault are installed

**_docker-container-purge-all_**\
&emsp;Remove all containers and delete volume mounts.

**_docker-container-purge-vault_**\
&emsp;Remove vault and delete volume mounts.

**_docker-container-remove-all_**\
&emsp;Remove all containers.

**_docker-container-remove-vault_**\
&emsp;Remove vault.

**_docker-container-restore-all_**\
&emsp;Run restic restore for all restic enabled containers.

**_docker-container-restore-vault_**\
&emsp;Run restic restore for vault if restic is enabled.

**_docker-container-setup-all_**\
&emsp;Run setup task for all containers.

**_docker-container-setup-vault_**\
&emsp;Run setup task for vault.

**_docker-container-unseal-vault_**\
&emsp;Run unseal task for vault.

**_never_**


## Dependencies

None.

## License

license (GPL-2.0-or-later, MIT, etc)

## Author

andif888
