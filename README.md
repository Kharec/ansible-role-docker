# ansible-role-docker

An Ansible role to install and configure Docker.

## Requirements

- Ansible 2.10 or higher
- Debian or Ubuntu-based distribution

## Dependencies

This role doesn't have any dependencies to other roles or collections.

## Playbook Example

A pretty complete playbook example to install Docker and configure it for multiple users, expose the Docker API, configure a Docker registry and create a Docker network.

```yaml
---
- hosts: docker_servers
  become: true
  roles:
    - ansible-role-docker
  vars:
    docker_users:
      - titou
      - antoine
    docker_data_dir: /opt/docker
    docker_expose_api: true
    docker_registry_url: "docker.io"
    docker_registry_email: "user@example.com"
    docker_networks:
      - name: "my-network"
        driver: "bridge"
        subnet: "172.20.0.0/16"
        gateway: "172.20.0.1"
        ip_range: "172.20.0.0/24"
```

**⚠️ SECURITY WARNING**: Exposing Docker API over TCP without TLS is insecure : consider using TLS or restrict access with firewall rules.

You can also use the role in its most simple form:

```yaml
---
- hosts: docker_servers
  become: true
  roles:
    - ansible-role-docker
```

## Variables

### Basic Configuration

| Variable                   | Required | Default     | Description                                                             |
| -------------------------- | -------- | ----------- | ----------------------------------------------------------------------- |
| `docker_api_host`          | No       | `127.0.0.1` | Host address to expose Docker API on (when `docker_expose_api` is true) |
| `docker_api_port`          | No       | `2375`      | Port to expose Docker API on (when `docker_expose_api` is true)         |
| `docker_arch`              | No       | `amd64`     | Architecture to install Docker for (e.g., "amd64", "arm64")             |
| `docker_data_dir`          | No       | ``          | Directory to store Docker volumes data                                  |
| `docker_expose_api`        | No       | `false`     | Whether to expose Docker API over TCP                                   |
| `docker_networks`          | No       | `[]`        | List of networks to create (dicts with name, driver, subnet, etc.)      |
| `docker_registry_email`    | No       | ``          | Registry email (mutually exclusive with `docker_registry_username`)     |
| `docker_registry_url`      | No       | ``          | Docker registry URL (registry authentication is skipped if not set)     |
| `docker_registry_username` | No       | ``          | Registry username (mutually exclusive with `docker_registry_email`)     |
| `docker_users`             | No       | `[]`        | List of users to be added to the `docker` group                         |

**Note**: When using registry authentication, the registry password must be provided via the `DOCKER_REGISTRY_PASSWORD` environment variable.

## License

This project is licensed under the GNU General Public License v3.0 or later (GPLv3+). See the [LICENSE](LICENSE) file for details.
