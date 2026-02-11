# ansible-role-minio-podman

An Ansible role to install and configure MinIO as a Podman container. This role sets up MinIO, a high-performance, S3-compatible
object storage system, running as a user-level Podman container with proper systemd integration for persistence and auto-restart
capabilities.

## Features

- Creates a dedicated `minio` system user with proper permissions
- Installs Podman container engine
- Pulls the MinIO image from Quay.io
- Enables systemd lingering for user-level services to survive logout
- Configures UID/GID mapping for the minio user
- Sets up MinIO data and certificate directories
- Deploys MinIO container with automatic restart policy
- Exposes MinIO API (port 9000) and Console (port 9001)
- Supports MinIO license key injection

## Role Variables

Available variables are listed below, along with their default values (see `defaults/main.yml`):

### User and Group Configuration

```yaml
minio_podman_group: "minio"
```

The system group for the MinIO user.

```yaml
minio_podman_user: "minio"
```

The system user that will run the MinIO container.

```yaml
minio_podman_shell: "/usr/bin/bash"
```

The shell for the MinIO user. Must be an interactive shell since the user needs to run Podman commands.

### Container Configuration

```yaml
minio_podman_image: "quay.io/minio/aistor/minio:latest"
```

The MinIO Podman image to use.

```yaml
minio_podman_pod_name: "minio"
```

The name of the Podman container.

### MinIO Credentials

```yaml
minio_podman_root_user: "minioadmin"
```

The root user for MinIO. This is the user that will be used to log in to the MinIO web interface and CLI.

```yaml
minio_podman_root_password: "minioadmin"
```

The root password for MinIO. This is the password that will be used to log in to the MinIO web interface and CLI.

### Minio-License

```yaml
minio_podman_license_key: ""
```

**Required.** The MinIO license key. This must be provided for the role to run successfully.

## Dependencies

This role depends on the `containers.podman` collection from Ansible Galaxy, which is automatically handled through collection
dependencies.

## Example Playbook

```yaml
---
- hosts: storage_servers
  become: true
  roles:
    - role: fgierlinger.minio_podman
      vars:
        minio_podman_license_key: "{{ vault_minio_podman_license_key }}"

---
- hosts: storage_servers
  become: true
  roles:
    - role: fgierlinger.minio_podman
      vars:
        minio_podman_license_key: "{{ vault_minio_podman_license_key }}"
        minio_podman_root_user:
```

## License

MIT

## Author

fgierlinger
