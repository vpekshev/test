# Infrastructure-Ansible

## Ansible setup
```bash
ansible-galaxy collection install community.general
ansible-galaxy collection install community.hashi_vault
```

## Playbooks

```bash
ansible-playbook hostpapa-general.yml -i inventories/shared --check
ansible-playbook hostpapa-infra.yml -i inventories/infra --check
ansible-playbook ddb.yml -i inventories/infra --check
VAULT_TOKEN=yourtoken ansible-playbook rotate_root.yml -i inventories/shared
VAULT_TOKEN=yourtoken ansible-playbook rotate_root.yml -i inventories/infra
```

You do need an SSH agent setup for these playbooks:

```bash
eval `ssh-agent`
ssh-add
ansible-playbook ubersmith.yml -i inventories/infra --check
ansible-playbook hostpapa-restricted-infra.yml -i inventories/infra --check
```

Limited access playbooks:

```bash
ansible-playbook vhi-certificates.yml -i inventories/vhi --check
```

## Development Environment

A [VS Code](https://code.visualstudio.com/) [development container](https://code.visualstudio.com/docs/devcontainers/containers) configuration is included. The development container includes Ansible as well as various linting tools/extensions for VS Code to make development easier.

To use the development container you will need:

- [VS Code](https://code.visualstudio.com/download)
- [Docker](https://docs.docker.com/get-started/overview/).
  - For Windows/Mac it is recommended to use [Docker Desktop](https://www.docker.com/products/docker-desktop/)
  - For Linux just [Docker Engine](https://docs.docker.com/engine/install/) is required
- [The Dev Containers extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) for VS Code.

Once the requirements are installed, simply open the workspace file and you will be prompted to re-open the workspace in the development container.

### Playbook Debugging/Development

The development container configuration includes an Alma and Debian container running SSH which can be used to test playbooks without the possibility of breaking live hosts.

To run playbooks on the development containers specify the hosts manually:

```bash
# For Alma only
ansible-playbook example.yml -i alma, -u root

# For Debian only
ansible-playbook example.yml -i debian, -u root

# For both containers
ansible-playbook example.yml -i alma,debian -u root
```
