# Ansible Role: IRI

Installs and configures [IRI](https://github.com/iotaledger/iri).

## Getting Started

First, you need to install the `Ansible>=2.4`:

```bash
# Ubuntu
$ sudo apt install ansible
# macOS
$ brew install ansible
```

Change the `inventory`, `group_vars` and `host_vars` according to your settings.

To deploy the IRI, execute the following command:

```bash
$ ansible-playbook iri.yml
```

To overwrite the existing setting, execute the following command:

```bash
$ ansible-playbook iri.yml -e overwrite=yes
```
