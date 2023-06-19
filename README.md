# stolostron.rhacm Collection for Ansible
The stolostron.rhacm collection performs full flow of environment creation for Advanced Cluster Management (ACM)

## Included content

Name | Description
--- | ---
[stolostron.rhacm.ocp](docs/ocp.md)| Deploy standalone Openshift cluster/s on different cloud platforms.
[stolostron.rhacm.acm](docs/acm.md)| Deploy Advanced Cluster Management (ACM) platform on selected cluster.
[stolostron.rhacm.managed_cluster](docs/managed_cluster.md)| Deploy managed cluster/s on the hub, by the hub using hive engine.
[stolostron.rhacm.cluster_login](docs/cluster_login.md)| Create Openshift cluster login token to perform operations on the cluster.

## Using this collection

### Installing the Collection

Before using this collection, you need to install it with the Ansible Galaxy command-line tool:
```bash
ansible-galaxy collection install git+https://github.com/stolostron/ansible-collection.rhacm.git,main
```

You can also include it in a `requirements.yml` file and install it with `ansible-galaxy collection install -r requirements.yml`, using the format:
```yaml
---
collections:
  - name: https://github.com/stolostron/ansible-collection.rhacm.git
    type: git
    version: main
```

## Tested with Ansible

TBD

## Collection maintenance

The current maintainers are listed in the [OWNERS](OWNERS) file. If you have questions or need help, feel free to mention them in the proposals.

## Code of Conduct

We follow the [Ansible Code of Conduct](https://docs.ansible.com/ansible/devel/community/code_of_conduct.html) in all our interactions within this project.

If you encounter abusive behavior, please refer to the [policy violations](https://docs.ansible.com/ansible/devel/community/code_of_conduct.html#policy-violations) section of the Code for information on how to raise a complaint.
