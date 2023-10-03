# ACM Import Cluster

## Description
The `acm_import_cluster` role imports or detaches Openshift clusters into ACM Hub.

## Role default variables
#### State of the cluster
Create or destroy the cluster.  
The state could be 'present' or 'absent'.
```
state: present
```

#### Clusters to import
List of clusters to be imported into the ACM Hub.  
**Note 1** - By default, cluster name specified here will be searched across clusters deployed by `ocp` and `managed_openshift` roles.  
Once found, it will search for kubeconfig of that cluster and will use it during the import of the cluster into the Hub.  
**Note 2** - The `kubeconfig` parameter is optional. It should be used only when a cluster was not created by the `ocp` or `managed_openshift` roles.
**Note 3** - The `cloud_creds` parameter is optional. Specify it if cloud credentials of the cluster should be created in Hub.  
Will search credentials under `ocp` role credentials variable - `clusters_credentials`
```
acm_import_clusters:
  - name: cluster1

  - name: cluster2
    # Optional. Apply cloud credentials.
    # Taken from "clusters_credentials" var of "OCP" role.
    cloud_creds: aws-creds

  - name: cluster3
    # Optional. Define addon flags per each cluster.
    application_manager: false
    argocd: false
    policy_controller: true
    search_collector: true
    cert_policy_controller: false
    iam_policy_controller: true

  - name: cluster4
    # Optional. Specify kubeconfig if cluster was not deployed by `ocp` or `managed_openshift` roles
    kubeconfig: /path/to/kubeconfig
```

#### Import cluster global addon flags
Define addon flags for all imported clusters.
```
acm_import_cluster_application_manager: true
acm_import_cluster_argocd: false
acm_import_cluster_policy_controller: true
acm_import_cluster_search_collector: true
acm_import_cluster_cert_policy_controller: true
acm_import_cluster_iam_policy_controller: true
```
