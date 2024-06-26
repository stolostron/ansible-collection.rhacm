# ACM

## Description
The `acm` role prepares and deploys MultiClusterHub on a given OCP cluster.

## Role Variables
#### State of the cluster
Create or destroy the cluster.  
The state could be 'present' or 'absent'.
```
state: present
```

#### ACM version
Specify the version of ACM Hub that should be deployed.  
Use the following format for the version: "2.8", "2.7".
```
acm_version: "2.8"
```

#### Snapshot
Optional. In order to deploy specific snapshot of ACM version, specify the snapshot.  
Example of snapshot: `2.7.2-DOWNSTREAM-2023-03-02-21-33-34`
**Note** - If snapshot is not specified, latest snapshot for chosen version will be selected automatically by the playbook.
```
snapshot:
```

#### ACM namespace
The namespace that should be used for ACM MCH deployment.  
```
acm_namespace: open-cluster-management
```

#### ACM components to enable/disable
Define which components should be enabled/disabled during deployment
```
acm_components:
  - name: app-lifecycle
    enabled: true
  - name: cluster-lifecycle
    enabled: true
  - name: cluster-permission
    enabled: true
  - name: console
    enabled: true
  - name: grc
    enabled: true
  - name: insights
    enabled: true
  - name: multicluster-engine
    enabled: true
  - name: multicluster-observability
    enabled: true
  - name: search
    enabled: false
  - name: submariner-addon
    enabled: true
  - name: volsync
    enabled: true
  - name: cluster-backup
    enabled: true
```

#### Deploy testing environment
If it's "false", ACM will be deployed from official RH registry 
If it's "true", ACM will be deployed from downstream testing registry
```
deploy_test_env: false
```

#### Registry secrets
During deployment of downstream MultiClusterHub, an access to internal registry is required.  
For that we need to provide registry secrets.  
The secrets will be included into the main `pull-secret` secret within ocp cluster.
```
registry_secrets:
  - name: "quay.io:443"
    user: <username>
    pass: <password>
  - name: "brew.registry.redhat.io"
    user: <username>
    pass: <password>
```

#### ACM registry mirror
ACM registry that is used to fetch the downstream images from.
```
acm_registry_mirror: quay.io:443/acm-d
```

#### Registry query url
The url is used to query the registry for the latest snapshot regarding provided ACM version.
```
registry_query_url: https://quay.io/api/v1/repository/acm-d/acm-custom-registry/tag/
```

#### ACM Catalog Sources
Catalog Sources used during ACM deployment to point to specific version of ACM.  
ACM and MCE are mandatory parts of ACM deployment.
```
catalog_sources:
  - type: acm
    catalog_name: acm-custom-registry
    display_name: Advanced Cluster Management
    catalog_ns: openshift-marketplace
  - type: mce
    catalog_name: mce-custom-registry
    display_name: MultiCluster Engine
    catalog_ns: openshift-marketplace
```

***
The role is able to work in one of the following states:
* Newly deployed cluster.  
  The cluster will be deployed by the [ocp](ocp.md) role and generate `clusters_details_file`.  
  Clusters info will be fetched from this file to generate the token for the next tasks.
* Existing cluster.  
  If the cluster already exists and was not deployed by us, token could be generated by providing the following environment variables prior playbook execution.
  ```bash
  export OC_CLUSTER_API=<ocp_cluster_api>
  export OC_CLUSTER_USER=<ocp_cluster_user>
  export OC_CLUSTER_PASS=<ocp_cluster_pass>

  ansible-playbook playbook.yml -e @config.yml
  ```

***
The variables could be applied to the playbook run, by saving them into a separate yml file and include the file during the playbook execution.  
Note the '@' sign, which is used to apply the variables located within the provided file.

```
ansible-playbook playbooks/acm.yml -e @/path/to/the/variable/file.yml
```
