# Installation 

Use these instructions to install your Atlassian product using the Helm charts. Make sure you have followed the [Prerequisites guide](PREREQUISITES.md) before you proceed with the installation.

#### 1. Add the Helm chart repository to your local Helm installation:

Add the Helm chart repository to your local Helm installation:

   `helm repo add atlassian-data-center https://atlassian-labs.github.io/data-center-helm-charts`
   
   `helm repo update`


#### 2. Create and update the `values.yaml` file of the product you’re installing:

Create the file:

   `helm show values atlassian-data-center/<product> > values.yaml`

Update the `values.yaml` based on what you provisioned as part of the prerequisites:
   
  * **Database**: If you provide all the required information via the `values.yaml` file, you will be able to bypass the database connectivity configuration during the product setup. To do this, update the `database` stanza in the `values.yaml` file:
   
    1. Update the `url` of the database. This is the Java Database Connectivity (JDBC) URL of the database to be used by the application. The format of this URL depends on the JDBC driver being used, but we provide some examples in [CONFIGURATION.md](CONFIGURATION.md).
    2. For Jira and Bitbucket: update the `driver` to be used. This is the Java class name of the JDBC driver to be used, for example `org.postgresql.Driver`.
    3. For Jira and Confluence: update the database `type`. This is the type of the database, for example `mssql`. 
    4. Update the `secretName` that is under `credentials`. You can provide the database username and password via a Kubernetes secret, with the secret name specified here.

  * **Ingress controller**: The Helm charts provide a template for an ingress resource. To use the template update the `ingress` stanza in the `values.yaml` file: 
  
    1. Change the `create` value to `true`
    2. Add a value to `host`. The ingress rules will apply to the host you provide. Hosts can be precise matches (for example “foo.bar.com”) or a wildcard (for example “*.foo.com”).  
      
  * **Persistent storage**: Each Data Center pod has its own `local-home` volume, and each pod within a cluster can utilize a single `shared-home` volume. By default, the Helm charts will configure all of these volumes as `emptyDir` volumes, but this is suitable only for running a single Data Center node for evaluation purposes. Proper volume management needs to be configured in order for the data to survive restarts, and for multi-node Data Center clusters to operate correctly.
     * For more details, please refer to the **Volumes** section of the [configuration guide](CONFIGURATION.md).
     * You can also refer to our examples of AWS storage: [Local storage - utilizing AWS EBS-backed volumes](docs/examples/storage/aws/LOCAL_STORAGE.md) and [Shared storage - utilizing AWS EFS-backed filesystem](docs/examples/storage/aws/SHARED_STORAGE.md)    
    * Bitbucket needs a dedicated NFS server providing persistence for a shared home. Prior to installing the Helm chart, a suitable NFS shared storage solution must be provisioned. The exact details of this resource will be highly site-specific, but you can use this example as a guide: [Implementation of an NFS Server for Bitbucket](docs/examples/storage/nfs/NFS.md)


#### 3. Install your chosen product: 

   `helm install <release-name> atlassian-data-center/<product> --namespace <namespace> --version <chart-version> --values values.yaml`

   * `<release-name>` is the name of your deployment and is up to you, or you can use `--generate-name`.

   * `<product>` can be jira, confluence, bitbucket or crowd.

   * `<namespace>` is optional. You can use namespaces to organize clusters into virtual sub-clusters.

   * `<chart-version>` is optional, and can be omitted if you just want to use the latest version of the chart.

   * `values.yaml` is optional and contains your site-specific configuration information. If omitted, the chart config default will be used.

   * Add `--wait` if you wish the installation command to block until all of the deployed Kubernetes resources are ready, but be aware that this may be waiting for several minutes if anything is mis-configured.


#### 4. Test your deployed product 

Make sure the service pod/s are running, then test your deployed product:

   `helm test <release-name> --logs -n --namespace <namespace>`

   * This will run some basic smoke tests against the deployed release.
   * If any of these tests fail, it is likely that the deployment was not successful. Please check the status of the deployed resources for any obvious errors that may have caused the failure.

#### 5. Complete your product setup 

Open your product in a web browser. If your product is Bitbucket your setup is complete and you will get straight to the login page. For the rest of the products, complete the setup according to the instructions you will find on the product’s web page. 

## Uninstall 
#### Uninstall your chosen product: 
 `helm uninstall <release-name> atlassian-data-center/<product>`

   * `<release-name>` is the name you chose for your deployment

   * `<product>` can be jira, confluence, bitbucket or crowd

***

* Continue to the [operation guide](OPERATION.md)
* Go back to the [prerequisites](PREREQUISITES.md) 
* Dive deeper into the [configuration](CONFIGURATION.md) options 
* Go back to [README.md](../README.md)
