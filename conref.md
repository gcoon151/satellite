---


copyright:
  years: 2020, 2024
lastupdated: "2024-11-05"

keywords: satellite, hybrid, multicloud

subcollection: satellite

content-type: conref

---

# Content references for Satellite
{: #conref-satellite}

## Understanding which ODF storage template to use
{: #compare-odf}

You have two options when deploying OpenShift Data Foundation to your {{site.data.keyword.satelliteshort}} clusters, which are the `odf-local` and `odf-remote` storage templates.

- [`odf-local`](/docs/satellite?topic=satellite-storage-odf-local): Choose this template when you have local storage available to your worker nodes. If your storage volumes are visible when running `lsblk`, you can use these disks when deploying ODF if they are raw and unformatted.
- [`odf-remote`](/docs/satellite?topic=satellite-storage-odf-remote): Choose this template if you have a CSI driver installed in your cluster. For example, the `azuredisk-csi-driver` driver. You can use the CSI driver to dynamically provision storage volumes when deploying ODF.



## Can I estimate my costs?
{: #cost-estimate}
{: faq}

When you create a resource such as a location or cluster, you can review a cost estimate in the **Summary** pane of the console. For other types of estimates, see [Estimating your costs](/docs/account?topic=account-cost#cost).

Keep in mind that some charges are not reflected in the estimate, such as the costs for your underlying infrastructure.

## Can I view and control my current usage?
{: #usage}
{: faq}
 

See [View your usage](/docs/account?topic=account-viewingusage#viewingusage) and [Set spending notifications](/docs/account?topic=account-spending) for general {{site.data.keyword.cloud_notm}} account guidance.


## Creating an assignment in the CLI
{: #assignment-create-cli}
{: cli}

1. List your storage configurations and make a note of the storage configuration that you want to assign to your clusters.
    ```sh
    ibmcloud sat storage config ls
    ```
    {: pre}

1. Get the ID of the cluster, cluster group, or service that you want to assign storage to. 

    To make sure that your cluster is registered with {{site.data.keyword.satelliteshort}} Config or to create groups, see [Setting up clusters to use with {{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-setup-clusters-satconfig).
    {: tip}
    
    Example command to list cluster groups.
    
    ```sh
    ibmcloud sat group ls
    ```
    {: pre}

    Example command to list clusters.
    
    ```sh
    ibmcloud oc cluster ls --provider satellite
    ```
    {: pre}
    
    Example command to list {{site.data.keyword.satelliteshort}} services.
    
    ```sh
    ibmcloud sat service ls --location <location>
    ```
    {: pre}

1. Assign your storage configuration to the cluster, group, or service that you retrieved earlier. For more information, see the `ibmcloud sat storage assignment create` [command](/docs/satellite?topic=satellite-satellite-cli-reference#storage-assignment-create-cli).

    Example command to assign a configuration to a cluster group.
    ```sh
    ibmcloud sat storage assignment create --group GROUP --config CONFIG --name NAME
    ```
    {: pre}

    Example command to assign a configuration to a cluster.
    ```sh
    ibmcloud sat storage assignment create --cluster CLUSTER --config CONFIG --name NAME
    ```
    {: pre}

    Example command to assign a configuration to a service cluster.
    ```sh
    ibmcloud sat storage assignment create --service-cluster-id CLUSTER --config CONFIG --name NAME
    ```
    {: pre}

1. Verify that your assignment is created.
    ```sh
    ibmcloud sat storage assignment ls (--cluster CLUSTER | --config CONFIG | --location LOCATION | --service-cluster-id CLUSTER)
    ```
    {: pre}
    
## Manually upgrading configurations in the CLI
{: #configuration-upgrade-manual-cli}
{: cli}

You can upgrade your {{site.data.keyword.satelliteshort}} storage configurations to get the latest storage template revision within the same major version. 

1. List your {{site.data.keyword.satelliteshort}} storage configurations, make a note of the {{site.data.keyword.satelliteshort}} configuration you want to upgrade.
    ```sh
    ibmcloud sat storage config ls
    ```
    {: pre}

1. Upgrade the {{site.data.keyword.satelliteshort}} configuration. Note, only the configuration is updated. If you want to upgrade the assignments that use this configuration, you can specify the `--include-assignments` option or you can manually update each assignment using the `assignment update` command.

    Example command to upgrade a configuration to the latest revision.
    ```sh
    ibmcloud sat storage config upgrade --config CONFIG [--include-assignments]
    ```
    {: pre}

    Example command to upgrade a configuration and it's associated assignments to the latest revision.
    ```sh
    ibmcloud sat storage config upgrade --config CONFIG --include-assignments
    ```
    {: pre}
    
## Manually updating assignments in the CLI
{: #assignment-update-cli}
{: cli}

Update an assignment to rename your assignment or assign it to new clusters or cluster groups.


1. List your assignments, make a note of the {{site.data.keyword.satelliteshort}} assignment you want to update and the clusters or cluster groups included in the assignment.
    ```sh
    ibmcloud sat storage assignment ls
    ```
    {: pre}

1. Update the assignment.

    Example command to update an assignment.
    ```sh
    ibmcloud sat storage assignment update --assignment ASSIGNMENT [--group GROUP ...] [--name NAME]
    ```
    {: pre}

    Example command to update assignment name and assign different cluster groups.
    
    ```sh
    ibmcloud sat storage assignment update --assignment ASSIGNMENT --name new-name --group group-1 --group group-2 --group group-3
    ```
    {: pre}
    

    
## Manually upgrading assignments in the CLI
{: #assignment-upgrade-cli}
{: cli}

Upgrade an assignment to use the latest storage template revision.

1. List your {{site.data.keyword.satelliteshort}} storage assignments, make a note of the {{site.data.keyword.satelliteshort}} assignment you want to upgrade.
    ```sh
    ibmcloud sat storage assignment ls
    ```
    {: pre}

1. List the {{site.data.keyword.satelliteshort}} storage templates to see the latest available versions.
    ```sh
    ibmcloud sat storage template ls
    ```
    {: pre}

1. Upgrade the {{site.data.keyword.satelliteshort}} assignment.

    Example command to upgrade an assignment.
    ```sh
    ibmcloud sat storage assignment upgrade --assignment ASSIGNMENT
    ```
    {: pre}



## Enabling automatic patch updates for configurations and assignments in the CLI
{: #assignment-autopatch-cli}
{: cli}

You can use the `sat storage assignment autopatch enable` CLI to enable automatic patch updates for your assignments. Enabling automatic patch updates applies the latest storage template revisions (patches) automatically. You must still apply major updates manually.

1. List your {{site.data.keyword.satelliteshort}} storage configurations. Make a note of the configuration ID. 

    ```sh
    ibmcloud sat storage assignment ls
    ```
    {: pre}

1. Run one of the following example commands to enable automatic patch updates for your configuration and its associated assignments. Enter the configuration ID that you retrieved in the previous step.

    Example command to enable automatic patch updates for an assignment.

    ```sh
    ibmcloud sat storage assignment autopatch enable --config CONFIG  (--all | --assignment ASSIGNMENT-ID [--assignment ASSIGNMENT-ID])
    ```
    {: pre}

    Example command to enable automatic patch updates for all storage assignments under a given configuration.

    ```sh
    ibmcloud sat storage assignment autopatch enable --config CONFIG --all
    ```
    {: pre}

    Example command to disable automatic patch updates for all assignments under a specific configuration.

    ```sh
    ibmcloud sat storage assignment autopatch disable --config CONFIG --all
    ```
    {: pre}

    Example command to disable automatic patch updates for an single assignment and a specific configuration.

    ```sh
    ibmcloud sat storage assignment autopatch disable --config CONFIG --assignment ASSIGNMENT-ID
    ```
    {: pre}

    Example command to disable automatic patch updates for an multiple assignment and a specific configuration.

    ```sh
    ibmcloud sat storage assignment autopatch disable --config CONFIG --assignment ASSIGNMENT-ID --assignment ASSIGNMENT-ID
    ```
    {: pre}



## Updating storage assignments in the console
{: #configuration-upgrade-console}
{: ui}

You can use the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external} to apply the latest patch updates to your assignments.

1. From the **Locations** page in the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, select your location.

1. Click the **Storage** tab to view your configurations.

1. Click the configuration you want to update.

1. Click information **Information** (i) icon to apply the latest revision or patch.

1. **Optional**: Enable automatic patch updates for your storage assignment. Enabling automatic patch updates ensures that your assignment always has the latest security fixes.

If you enable automatic patch updates, you must still apply major updates manually.
{: note}


    


## Creating a storage assignment in the API
{: #assignment-create-api}
{: api}

1. Copy one of the following example requests. 

    Example request to assign a [configuration to a cluster](https://cloud.ibm.com/apidocs/kubernetes/containers-v1-v2#createassignmentbycluster-satellite){: external}.
    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createAssignmentByCluster" -H "accept: application/json" -H "Authorization: Bearer TOKEN" -H "Content-Type: application/json" -d "{ \"channelName\": \"CONFIGURATION-NAME\", \"cluster\": \"CLUSTER-ID\", \"controller\": \"LOCATION-ID\", \"name\": \"ASSIGNMENT-NAME\"}"
    ```
    {: pre}
    
    Example request to [assign configuration to a cluster group](https://cloud.ibm.com/apidocs/kubernetes/containers-v1-v2#createassignment-satellite){: external}.
    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createAssignment" -H "accept: application/json" -H "Authorization: Bearer TOKEN" -H "Content-Type: application/json" -d "{ \"channelName\": \"CONFIGURATION-NAME\", \"cluster\": \"string\", \"groups\": [ \"CLUSTER-GROUP\" ], \"name\": \"ASSIGNMENT-NAME\"}"
    ```
    {: pre}
    
1. Replace the variables with your details and run the request.

1. Verify the assignment was created by listing your assignments.

    ```sh
    curl -X GET "https://containers.cloud.ibm.com/global/v2/storage/satellite/getAssignments" -H "accept: application/json" -H "Authorization: Bearer TOKEN"
    ```
    {: pre}

## Updating a storage assignment in the API
{: #assignment-update-api}
{: api}

You can use the [`/v2/storage/satellite/updateAssignment`](https://cloud.ibm.com/apidocs/kubernetes/containers-v1-v2#updateassignment-satellite){: external} API to update your assignments with new clusters or cluster groups.

1. Copy the following example request and replace the variables for the cluster groups and assignments that you want to update.
    ```sh
    curl -X PATCH "https://containers.cloud.ibm.com/global/v2/storage/satellite/updateAssignment" -H "accept: application/json" -H "Authorization: Bearer TOKEN" -H "Content-Type: application/json" -d "{ \"groups\": [ \"CLUSTER-GROUPS\" ], \"name\": \"ASSIGNMENT-NAME\", \"updateConfigVersion\": true, \"uuid\": \"ASSIGNMENT-ID\"}"
    ```
    {: pre}
    
1. Run the request.

1. Get the details of you assignment to verify the update.
    ```txt
    curl -X GET "https://containers.cloud.ibm.com/global/v2/storage/satellite/getAssignment?uuid=ASSIGNMENT-ID" -H "accept: application/json" -H "Authorization: Bearer TOKEN"
    ```
    {: pre}


## Upgrading a configuration and assignments in the API
{: #assignment-upgrade-api}
{: api}

You can use the [`/v2/storage/satellite/updateAssignment`](https://cloud.ibm.com/apidocs/kubernetes/containers-v1-v2#updateassignment-storage){: external} API to update your assignments with new clusters or cluster groups. Set `updateConfigVersion` to `true` to apply the revision update.

1. Copy the following example request and replace the variables for the cluster groups and assignments that you want to update.
    ```sh
    curl -X PATCH "https://containers.cloud.ibm.com/global/v2/storage/satellite/updateAssignment" -H "accept: application/json" -H "Authorization: Bearer TOKEN" -H "Content-Type: application/json" -d "{ \"groups\": [ \"CLUSTER-GROUPS\" ], \"name\": \"ASSIGNMENT-NAME\", \"updateConfigVersion\": true, \"uuid\": \"ASSIGNMENT-ID\"}"
    ```
    {: pre}
    
1. Run the request.

1. Get the details of you assignment to verify the update.
    ```txt
    curl -X GET "https://containers.cloud.ibm.com/global/v2/storage/satellite/getAssignment?uuid=ASSIGNMENT-ID" -H "accept: application/json" -H "Authorization: Bearer TOKEN"
    ```
    {: pre}




## Enabling automatic patch updates for assignments in the API
{: #assignment-autopatch-api}
{: api}

You can use the `/v2/storage/satellite/setAssignmentAutoupgrade` API to enable automatic patch updates for your assignments. Enabling automatic patch updates applies the latest storage template revisions (patches) automatically. You must still apply major updates manually.

1. Copy the following example request and replace the variables for the cluster groups and assignments that you want to update.
    ```txt
    curl -X PATCH "https://containers.cloud.ibm.com/global/v2/storage/satellite/setAssignmentAutoupgrade" -H "accept: application/json" -H "Authorization: Bearer TOKEN" -H "Content-Type: application/json" -d { "config": "string", "controller": "string", "autopatch": boolean,"assignment" : { "all": boolean, "uuid": ["string", "string", ...], } }
    ```
    {: pre}
    
1. Run the request.

1. Get the details of you assignment to verify the upgrade.
    ```sh
    curl -X GET "https://containers.cloud.ibm.com/global/v2/storage/satellite/getAssignment?uuid=ASSIGNMENT-ID" -H "accept: application/json" -H "Authorization: Bearer TOKEN"
    ```
    {: pre}


    
## Removing a storage configuration from the console
{: #configuration-remove-console}
{: ui}

1. From the {{site.data.keyword.satelliteshort}} storage dashboard, select the storage configuration you want to delete.
1. Select **Actions** > **Delete**.
1. Enter the name of your storage configuration.
1. Select **Delete**.
