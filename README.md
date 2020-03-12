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

1. Modify the `inventory` file according to your settings.

Example:

```
[fullnode]
node ansible_host=192.168.0.201 ansible_user=ubuntu
```

| | Description |
|-|-|
| [fullnode] | The group name |
| node | The service name |
| ansible_host=192.168.0.201 | The hostname or the IP address of the service |
| ansible_user=ubuntu | The ID for SSH logging in to the service hardware |

2. Setup SSH public key to all your `remote worker`:

```bash
# copy ~/.ssh/id_rsa.pub to all remote worker.
$ ansible-playbook setup-fpga-boards.yml
```

3. Test the connection to these machines:

```bash
$ ansible-playbook ping-all.yml
```

4. Modify the variables in `group_vars/*` and `host_vars/*`.

Example:

| Assign RabbitMQ broker service hostname or IP address | File | Variable |
|:-:|-|-|
| IRI | [ansible-iri-playbook/group_vars/fullnode/iri.yml](https://github.com/DLTcollab/ansible-iri-playbook/tree/master/group_vars/fullnode/iri.yml) | iri_remote_broker_host |
| Remote Worker | [ansible-iri-playbook/group_vars/remote-worker/remote-worker.yml](https://github.com/DLTcollab/ansible-iri-playbook/blob/master/group_vars/remote-worker/remote-worker.yml) | remote_worker_broker |

5. Build the whole infrastructure:

The default setting is to activate the RabbitMQ first, then the IRI and the remote workers.
Beware that the RabbitMQ must be the first one, otherwise the connection between the services can not be set up.

```bash
$ ansible-playbook iri.yml
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
