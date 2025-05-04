# Base Ansible Configuration

This repository contains Ansible automation for setting up and configuring a base infrastructure node with various services including GitLab runners, Docker registry, Traefik, and NFS server configuration.

## Overview

This Ansible configuration automates the deployment and configuration of a base infrastructure node with the following components:

- NFS Server setup and configuration
- GitLab Runner configuration (multiple runners)
- Docker Registry with authentication
- Traefik configuration
- Promtail setup
- Docker environment configuration

## Prerequisites

- Ansible installed on the control node
- Target host running a Debian-based Linux distribution
- SSH access to the target host
- Required environment variables configured

### Required Environment Variables

```bash
DOMAIN              # Domain name for services
DOMAIN_OLD          # Legacy domain name if applicable
GLRUN01_TOKEN       # GitLab Runner 1 token
GLRUN02_TOKEN       # GitLab Runner 2 token
GLRUN03_TOKEN       # GitLab Runner 3 token
GLRUN10_TOKEN       # GitLab Runner 10 token
REGISTRY_HTPASSWD   # Docker registry authentication
VLAN_INFRA          # Infrastructure VLAN configuration
DOCKER_ENV          # Docker environment configuration
```

## Repository Structure

```
.
├── collections/
│   └── requirements.yml    # Ansible collection and role dependencies
├── playbooks/
│   └── main.yml           # Main playbook for base server setup
├── roles/
│   └── base_node/         # Main role for base infrastructure setup
│       ├── defaults/
│       ├── handlers/
│       └── tasks/
└── .gitlab-ci.yml         # GitLab CI/CD pipeline configuration
```

## Features

### NFS Server
- Installs and configures NFS kernel server
- Creates export directory with proper permissions
- Configures exports for specified VLAN subnet

### Docker Setup
- Uses external role `docker_common_setup` for Docker installation
- Configures Docker Compose environment
- Sets up various service containers

### Service Configuration
- GitLab Runners (4 instances with different configurations)
- Docker Registry with authentication
- Traefik reverse proxy
- Promtail log aggregation

## Installation

1. Clone the repository:
```bash
git clone https://github.com/InvalidIdentifier/base-ansible.git
```

2. Install required collections and roles:
```bash
ansible-galaxy install -r collections/requirements.yml
```

3. Configure your inventory file with the target host under the `basenode` group

4. Set up required environment variables

5. Run the playbook:
```bash
ansible-playbook playbooks/main.yml -i inventory
```

## CI/CD Pipeline

The repository includes a GitLab CI pipeline that:
- Sets up SSH authentication
- Installs required Ansible collections and roles
- Executes the playbook
- Sends notifications on successful deployment

## Dependencies

- community.general collection
- ansible.posix collection
- docker_common_setup role (from external repository)

## License

This project is licensed under the GNU General Public License v3.0 - see the [LICENSE](LICENSE) file for details.
