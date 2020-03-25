## Ansible IRI Playbook

Install and configure:

- [IRI](https://github.com/DLTcollab/iri)
- [RabbitMQ](https://hub.docker.com/_/rabbitmq)
- [Remote Worker](https://github.com/DLTcollab/dcurl)

### Prerequisite

Install the `Ansible>=2.4`:

#### Ubuntu
```bash
$ sudo apt install ansible
```

#### macOS
```bash
$ brew install ansible
```

Clone this playbook:

```bash
$ git clone git@github.com:DLTcollab/ansible-iri-playbook.git
```

Install the third-party roles:

```bash
$ ansible-galaxy install --roles-path ~/.ansible/roles -r requirements.yml
```

### Getting Started

#### Fullnode, Broker and Remote Worker

1. Add your SSH public key to your `fullnode`, `broker` and `remote-worker`, and make sure you can access them via SSH.

For quickly adding the SSH public key to multiple remote workers, please check [Optional: Set up Remote Worker SSH Public Key](#optional-set-up-remote-worker-ssh-public-key).

2. According to your SSH settings, add `fullnode`, `broker` and `remote-worker` to `inventory`.

For example:

```
[fullnode]
node ansible_host=192.168.0.201 ansible_user=ubuntu
```

| Keyword                    | Description                                       |
| -------------------------- | ------------------------------------------------- |
| [fullnode]                 | The group name                                    |
| node                       | The service name                                  |
| ansible_host=192.168.0.201 | The hostname or the IP address of the service     |
| ansible_user=ubuntu        | The ID for SSH logging in to the service hardware |

3. Test the connection:

```bash
$ ansible-playbook ping-all.yml
```

#### Build Infrastructure

1. Modify the variables in `group_vars/*` and `host_vars/*` as you need.

Example:

| Assign RabbitMQ broker service hostname or IP address | File                                                                                                                                                                        | Variable               |
| :---------------------------------------------------: | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------- |
|                          IRI                          | [ansible-iri-playbook/group_vars/fullnode/iri.yml](https://github.com/DLTcollab/ansible-iri-playbook/tree/master/group_vars/fullnode/iri.yml)                               | iri_remote_broker_host |
|                     Remote Worker                     | [ansible-iri-playbook/group_vars/remote-worker/remote-worker.yml](https://github.com/DLTcollab/ansible-iri-playbook/blob/master/group_vars/remote-worker/remote-worker.yml) | remote_worker_broker   |

2. Build the whole infrastructure:

The default setting is to activate the RabbitMQ first, then the IRI and the remote workers.
Beware that the RabbitMQ must be the first one, otherwise the connection between the services can not be set up.

```bash
$ ansible-playbook iri.yml
```

#### Optional: Set up Remote Worker SSH Public Key

If you are using [de10nano-image](https://github.com/DLTcollab/de10nano-image) system image to remote-worker, you could use the playbook to help you set up SSH access.

1. Add your remote-worker to `[remote-worker]` group in `inventory`.

2. Copy public key(`~/.ssh/id_rsa.pub`) to all `remote worker`:

```bash
$ ansible-playbook setup-fpga-boards.yml
```

3. Test the connection:

```bash
$ ansible-playbook --limit remote-worker ping-all.yml
```

### FAQ

After modifying the fullnode variables, overwrite the existing setting:

```bash
$ ansible-playbook iri.yml -e overwrite=yes
```

To change service status:

```bash
$ ansible-playbook manage.yml -e systemd_status=[status]
```

The acceptable **[status]** are

- `started`
- `restarted`
- `stopped`

You could use the [--limit](https://ansible-tips-and-tricks.readthedocs.io/en/latest/ansible/commands/#limit-to-one-or-more-hosts) to change the service status against one or more members.

```bash
$ ansible-playbook manage.yml --limit [group/host] -e systemd_status=[status]
```

The acceptable **[group/host]** are

- `fullnode`
- `broker`
- `remote-worker`
- `node`
- `remote-worker-1`
- `remote-worker-2`
- `remote-worker-3`
- `remote-worker-4`
