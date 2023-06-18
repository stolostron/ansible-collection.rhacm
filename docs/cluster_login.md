# CLUSTER_LOGIN

## Description
The `cluster_login` role generates login token for specified Openshift cluster.  
The role could work in one of the following states:
* Fetch cluster details to generate the token from the `clusters_details_file`, which is created by the [ocp](ocp.md) role during creation of openshift clusters.
* Fetch cluster details to generate the token by using the following environment variables exported by the user prior executing the playbook:
  ```bash
  export OC_CLUSTER_API=<ocp_cluster_api>
  export OC_CLUSTER_USER=<ocp_cluster_user>
  export OC_CLUSTER_PASS=<ocp_cluster_pass>
  ```

## Role Variables
#### Cluster name
Specify the name of the cluster to generate the login token for.  
If the variable is not defined, the first cluster from the details file will be taken.
```
cluster_name
```

#### Clusters details file
The name of the file that contains details of the clusters.
```
clusters_details_file: clusters_details.yml
```

#### OCP assets dir
Path to directory that contains clusters assets.
```
ocp_assets_dir: logs/ocp_assets
```
