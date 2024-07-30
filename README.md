# stolostron.rhacm Collection for Ansible
The stolostron.rhacm collection performs full flow of environment creation for Advanced Cluster Management (ACM)

## Included content

Name | Description
--- | ---
[stolostron.rhacm.ocp](docs/ocp.md)| Deploy standalone Openshift cluster/s on different cloud platforms.
[stolostron.rhacm.acm](docs/acm.md)| Deploy Advanced Cluster Management (ACM) platform on selected cluster.
[stolostron.rhacm.acm_hive_cluster](docs/acm_hive_cluster.md)| Deploy ACM hive cluster/s on the hub, by the hub using hive engine.
[stolostron.rhacm.acm_import_cluster](docs/acm_import_cluster.md)| Import Openshift cluster/s into the hub.
[stolostron.rhacm.managed_openshift](docs/managed_openshift.md) | Deploy Managed Openshift cluster/s.
[stolostron.rhacm.cluster_login](docs/cluster_login.md)| Create Openshift cluster login token to perform operations on the cluster.

## Using this collection

### Installing the Collection

Before using this collection, you need to install it with the Ansible Galaxy command-line tool:
```bash
ansible-galaxy collection install git+https://github.com/stolostron/ansible-collection.rhacm.git,v0.0.5
```

You can also include it in a `requirements.yml` file and install it with `ansible-galaxy collection install -r requirements.yml`, using the format:
```yaml
---
collections:
  - name: https://github.com/stolostron/ansible-collection.rhacm.git
    type: git
    version: v0.0.5
```

The `kubernetes` pip package needs to be installed as it required by some of the collection roles.  
Install the package from the `requirement.txt` file or add the package to your own `requirements.txt` file.
```bash
pip install requirements.txt
```

### Prepare config file as input for execution

Deployment of ACM based environment very generic.  
It could be customized to meet various requirements.  
Fetch the sample config [file](docs/config-sample.yml) and apply the changes that relevant for your environment.

### Create playbook to execute the required roles

Create playbook/s to call the relevant roles to create required resources.  
The following example includes the following roles to create required environment: ocp, acm, acm_hive_cluster.
```yaml
- hosts: localhost
  connection: local
  gather_facts: false
  collections:
    - stolostron.rhacm
  roles:
    - ocp
    - acm
    - acm_hive_cluster
```
Check for playbooks examples [here](docs/playbooks_examples/).

### Execute deployment of the environment

Perform environment deployment by using config file prepared earlier.
```bash
ansible-playbook playbook.yml -e @config.yml
```

### Playbooks resources state

Resources defined within the roles could be created or deleted.  
Creation or deletion of the resources is done by cross roles variable called - `state`.  
The variable `state` accept the following values - `present` or `absent`.  
This variable could be provided in three possible places: config file, playbook or via the cli.

Playbook example:
```
- hosts: localhost
  connection: local
  gather_facts: false
  vars:
    state: present
  roles:
    - ocp
    - acm
```

Command line example:
```
ansible-playbook playbooks/play.yml -e @config.yml -e state=absent
```

## Tested with Ansible

TBD

## Collection maintenance

The current maintainers are listed in the [OWNERS](OWNERS) file. If you have questions or need help, feel free to mention them in the proposals.

## Code of Conduct

We follow the [Ansible Code of Conduct](https://docs.ansible.com/ansible/devel/community/code_of_conduct.html) in all our interactions within this project.

If you encounter abusive behavior, please refer to the [policy violations](https://docs.ansible.com/ansible/devel/community/code_of_conduct.html#policy-violations) section of the Code for information on how to raise a complaint.
