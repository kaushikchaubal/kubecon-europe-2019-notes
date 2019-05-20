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

### Mission 
* To make writing k8s extensions less arcane

### Pre-requisites
* Install go (1.11 or above). Installation link: https://golang.org/dl/
* Install Kubebuilder. Installation link: https://book.kubebuilder.io/quick-start.html
* Install gcloud. Installation link: https://cloud.google.com/sdk/docs/quickstart-macos 

### Theory Intro
* A **controller** is a loop that reads desired state, observed cluster state and external state
* An **operator** is a controller that encodes human operational knowledge: how do i run and manage a specific piece of complex software
* All operators are controllers but not all controllers are operators
* Structure of the 4 sections:
    - Learn
    - Try
    - Review

### Section 1: What's KubeBuilder

#### Learn
* Tool to build custom contollers and operators
* Consists of two parts
    - controller-runtime
    - controller-tools
* BUilding a Guuestbook operator (https://cloud.google.com/kubernetes-engine/docs/tutorials/guestbook)
* Git repo: https://github.com/DirectXMan12/kubebuilder-workshops 
* Worth cheking out: https://book.kubebuilder.io/quick-start.html 

#### Try

Run this locally on your machine:
```bash 
# Assumption here is that you already have kubebuilder installed locally 
git clone https://github.com/DirectXMan12/kubebuilder-workshops.git
cd kubebuilder-workshops/
kubebuilder init --project-version 2 --domain kctest
```

#### Review
* Initialise a new KubeBuilder project
* Initialise a new Go module for our project
* Generate deployment config for running for k8s
* Configure the API Suffice

### Section 2: Creating a new API in K8s

#### Learn
* Every K8s API consists of:
    - Spec 
    - Status 
    - Metadata
    - List 
* Create an API group named webapp.<my-domain>
* Create an API version webapp.<my-domain>.v1
* Add a new Kind GeuestBook to that group and a controller for it

### Try

Run this locally on your machine:
```bash
kubebuilder create api --group webapp --version v1 --kind Guestbook
make generate manifests
```

#### Review
* We created a custom spec eg: FrontEndSpec (custom struct), RedisName (String), UseDNS (bool)
* Have a look at the following commits:
    - Scafold Guestbook API: https://github.com/DirectXMan12/kubebuilder-workshops/commit/0082f927952a7c9204a9e4ee2db7da9c8055c385
    - Scafold Redis API: https://github.com/DirectXMan12/kubebuilder-workshops/commit/8316e1df09a5c1ecfad68ce5317e5700345b402d
    - Add Frontend configuration: https://github.com/DirectXMan12/kubebuilder-workshops/commit/c6ff89a900de9f540ec233edc26b88cdf656db9e 
    - Add API Status: https://github.com/DirectXMan12/kubebuilder-workshops/commit/78a60ce02546f47153090f852d659f43ee85deff
    - Add Redis API Configuration: https://github.com/DirectXMan12/kubebuilder-workshops/commit/bc6ed3da25f9d64065bb95b56d675210d23782c0 


### Section 3: Interlude - groups, versions and kinds

#### Learn
* **API Group** is a collection of related API types
* Each API type is a **Kind**
* Each API group has one or more **API Versions**
* Each Kind is used in at least one **Resource**
* Each Go type coresponds to a particular **Group-Version-Kind**
* Read, Reconcile, Repeat
    - Read our root object
    - Fetch other objects we care about
    - Ensure those objects are in the right state
    - Write
* Clients, Schemes, Requests
    - Each reconsiler takes a request, returns a result and error
    - Requests can use client.Get to turn the request tinto an actual object and CreateOrUpdate to ensure that an object is up=to-date
    - Clients use a Scheme to associate Go types with Kinds
* What does that actually mean?
    - Fetch our GuestBook
    - Ensure desired state
    - Update status with observed state

#### Try

Run this locally on your machine - pointing to gcloud:
```bash 
# Ensure you are logged into gcloud (and pointing to the correct config)
gcloud config configurations list

# If not, follow the steps as follows
gcloud auth login
gcloud container clusters get-credentials standard-cluster-1 --zone us-west1-a --project kbuilder-kubecon19-bcn-4315

# Edit the controller
kubectl create -f config/crd/bases
make run
kubectl create -f config/samples && kubectl describe guestbooks

# Locating your newly-create CRD
kubectl get crd
```

### Section 4: Idempotency
* Reconilers should be idempotent i.e needs to be done and have no side effects
* lways take actions based on the observed cluster and external state, not the event that triddered a recon
* Prefer writing logic in terms of 'ensure this is correct', not specifically create of update
* Use owner references to take care of delete for you
* Ensure desired state with CreateOrUpdate
* Set StateConditions to indicate health