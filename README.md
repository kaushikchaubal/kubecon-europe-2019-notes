## Anthos Workshop

### Setup

* https://codelabs.developers.google.com/codelabs/anthos-workshop/#0
* 4 nodes running on GKE
* 4 nodes running on GCP (as normal k8s cluster)

### Initial notes:
* K8s is orchestration for your pods (in one cluster)
* Anthos is orchestration for your cluster
* Also worth chcking out traffic director: https://cloud.google.com/traffic-director/ 
* Main point that Anthos is tryin to solve: Multi-cluster management problems

### Lab: Step 1,2,3 from codelab
* Creating a GKE cluster.
* Creating a remote cluster.
* Installing Anthos components.

### Lab: Step 4 from codelab. 

At the end of this:
* Registered a non-GKE Kubernetes cluster to GKE Hub.
* Reviewed GKE & non-GKE clusters and workloads through GKE Hub.
* Reviewed workloads running in various locations across all your clusters.
* Also, note the clusters are available inside GCP (https://console.cloud.google.com/kubernetes/list)

### Notes:
* Config-as-code: hybrid Anthos environment expressed in a secure on-prep code repo
* You can use fluentd for captuting logging in the distributed system: https://logz.io/blog/fluentd-tutorial/ 
* Useful to switch easily between different clusters: https://ahmet.im/blog/kubectx/ 

### Lab: Step 5 from codelab. 

At the end of this:
* Setup your config repository.
* Connected your clusters to the repo.
* Watched clusters come under compliance.
* Pushed configuration changes across clusters.
* Reviewed automated drift management.
* Pushed changes to only select clusters.

### Notes:
* Isitio helps with smart networking as a service mesh
* This can be done across multiple clusters
* Isitio has a control plane and data plane
* What are the options about how to run isitio runs in multiple clusters environment
    1. Single control plane can manage all data planes
    2. Both clusters can have it's own control plane
* Dual control plane
    * Low networking requirements
    * To enable mTLS across meshes/clusters
    * To effectivelty route between clusters
* We are going to run the hipster app: https://github.com/GoogleCloudPlatform/microservices-demo
* The hipster app runs 10 different microservices 
* This is going to use the ingress gateway (which is a Istio concept)
* Worth remembering that there are 'n' services meshes based on 'n' clusters
* Expose only the services that needs to be used across clusters
* Use 2 screenshots about how this looks like
* Use 1 picture to give example about how the end-to-end communication is taking place

### Lab: Step 6 from codelab. 

At the end of this:
* Deployed an app using a hybrid model across multiple clusters.
* Reviewed the mechanics of multi-cluster mesh patterns.
* Configured service discovery between clusters.
* Inspected Routing, Security & Service Discovery.
* Migrated the remote, non-GKE workloads to cloud-based GKE.

### Notes:
* Cloud Service Mesh (alpha) provides tools and insights to help you manage your workloads, not just your infrastructure
* GKE on Prem, GKE connect will be GA next month

### Lab: Step 7 from codelab. 

At the end of this:
* Reviewed workload topology and connections.
* Reviewed security suggestions for your services.
* Inspected service level metrics and telemetry.
* Defined and inspected Service Level Objectives.

### Final notes:
* Podcast for k8s: https://kubernetespodcast.com/ 

-----

## Writing Operators with Kubebuilder v2






