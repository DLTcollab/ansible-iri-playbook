## Ansible IRI Playbook

Installs and configures:

- [IRI](https://github.com/iotaledger/iri)
- [rabbitmq:management](https://hub.docker.com/_/rabbitmq)
- [PoW Remote Worker](https://github.com/DLTcollab/dcurl)

### Prerequisite

Install the `Ansible>=2.4`:

```bash
# Ubuntu
$ sudo apt install ansible
# macOS
$ brew install ansible
```

Clone this playbook:

```bash
$ git clone git@github.com:DLTcollab/ansible-iri-playbook.git
```

Install third-party roles:

```bash
$ ansible-galaxy install --roles-path ~/.ansible/roles -r requirements.yml
```

### Getting Started

#### Setup FPGA Remote worker

1. Modify the `inventory` according to your settings.

2. Setup SSH public key to all your `remote-worker`:

```bash
# copy ~/.ssh/id_rsa.pub to all remote-worker.
$ ansible-playbook setup-fpga-boards.yml
```

3. Test the connection to these machines:

```bash
$ ansible-playbook ping-all.yml
```

4. Modify the variables in `group_vars/*` and `host_vars/*`.

5. Build the whole infrastructure:

```bash
$ ansible-playbook iri.yml
```

### FAQ

After modify the fullnode variables, overwrite the existing setting:

```bash
$ ansible-playbook iri.yml -e overwrite=yes
```
