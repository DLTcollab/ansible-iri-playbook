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

#### Setup FPGA Remote Worker

1. Modify the `inventory` according to your settings.

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

```bash
$ ansible-playbook iri.yml
```

### FAQ

After modifying the fullnode variables, overwrite the existing setting:

```bash
$ ansible-playbook iri.yml -e overwrite=yes
```
