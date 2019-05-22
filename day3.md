## Keynote

### Theme:
* K8s is a platform for Creating Platforms
* It is not the end, it is what we use to provide the user

### Talk 1: How Spotify Accidentally Deleted All its Kube Clusters with No User Impact - David Xia
* Compute Environment - uses GCP
    - Teams that build infra: Infra team (cluster operators)
    - Teams that use infra: Feature team (cluster users)
* 3 production clusters (backing up every one hour)
* Story 1: Deleted US accidently
    - created a 'test' cluster
    - opened two tabs and did some test it
    - deleted the wrong cluster
* How do I make it stop? You don't!
* Cluster Restoration
    - Took 3.25 hours to restore it
    - Bugs in cluster restoration scripts
    - incomplete documentation
* Used TerraForm to create Infra as code (to stop the incident happen again)
* Story 2: Deleted US accidently
    - Import the stage file
    - Ran a review file
    - Cluster user created a PR that declared 
    - Unknowingly that deleted 

* Can't get any worse, right?
    - We try to recreate the cluster by merging the remaining PR
    - Cluster creation fails from lack of perms
    - we grany enough but different perms to make it work
    - caused Terraform's view of the clusters to change
* Developer Impact
    - One team had to create more non-k8s VMs
    - Oprator team had to update all the places they had hardcoded the old master IP
    - Everyone had to refresh the creds
* End user Impact
    - No results
* Why?
    - Planned for failure
        * Each team only migrate services partially to k8s
            - k8s usage was marked beta
            - Recommended teams to migrate only few services
        * The way services were registereed on k8s
            - Internal service discovery uses pod ips
            - Dont use the k8s service IP
            - Pools service endpoints and updates the service IP
        * Resulting failover to non-k8s instances
            - service discovery system was restarted
            - k8s pods removed from service discovery
            - clienfs only got a list of non-k8s services
    - Migrated large scale, complex infrastracture gradually
    - Culture of learning
* Best practices
    - Backed up the clusters
    - Codified the infra
    - Performed disaster recovery tests
    - Practice makes perfect!
* Next steps
    - k8s is now GA within Spotify
    - Manage conifg and workload distribution across many clusters
    - Create more redundancies

### Talk 2: Building a Bigger Tent: Cloud Native, Cultural Change and Complexity - Bob Quillin
* Golden age for developer
    - DevOps - how?
    - OpenSource - what?
    - Public cloud - where?
* GitLab developer survey: https://about.gitlab.com/developer-survey/2018/ 
* Building a better tent:
    - Open
    - Sustainable
    - Inclusivce
* Java libraries for writing microservices
    - https://helidon.io
    - https://github.com/oracle/helidon 

### Talk 3: A Journey to a Centralized, Globally Distributed Platform – Katie Gamanji
* Moving from fragmented platforms to one Global PLatform
    - Inconsistencies
        * Systems to serive live traffic
        * CMS tools
        * Visuals and design
* Stack
    - CDN provider: https://www.fastly.com/
    - Ingress endpoint: https://traefik.io/ 
    - Tectnonic: https://coreos.com/tectonic/ 
    - AWS as cloud provider
    - Infra deployed as Terraform
* Others open-source technologies used
    - Github
    - Quay (https://quay.io/)
    - DataDog
    - Helm
    - FluentD
* Origin latency: 5 different regions, 9 different clusters
* Self-service CI/CD (to further enable developers)
    - Teams require docker images
    - Stored in quay
    - Given a template using helm chart
    - Circle CI pipeline is triggered
* They have resource quotas
* RBAC rules
    - read-only for developers and admin teams
* Challenges:
    - Dev experience
    - Service continuity
    - Upgrades
    - China and Russia
    - Tracing, obsevability and sevice mesh

### Talk 4: What I Learned Running 10,000+ Kubernetes Clusters - Jason McGee
* How do make the day-to day activies more easier for the 25 person SRE team on the front line?
    - Create bots i.e. ChatOps
* How do we know what is deployed? and how to update it?
    - Pull based self-updating clusters
    - Flexible rule and label based configuratino
    - Geature fladded deployments
    - Dynamic inventory and change history
* A multi-cluster continuous delivery tool for Kubernetes: https://razee.io/

### Talk 5: Debunking the Myth: Kubernetes Storage is Hard - Saad Ali
* What about stateful apps?
    - Containers are inherently e[phemeral
    - Stateful apps need to persist bits across nodes
* K8s storage myths
    - It's hard
    - Don't run on k8s
* Reality: Storage is complicated
* Separation of concerns
    1. Select (what storage should i use?)
        * Object stores
        * SQL databases
        * Timeseries
        * File storage
        * NoSQL storage 
        * Messaging queues
        * Grouped into 2 categories: Data services v/s Block / File
    2. Deploy (how do I deploy and manage my storage?)
        * Managed - someone else deploys and manages
        * Unmanaged - you deploy and manage
        * Note: You do not have to deploy your storage on k8s to use it in k8s
        * Storage deployed on top of k8s is just another stateful application
        * Use an operator to deploy applications with cokmplicated life cycles
            - A Storage Orchestrator for Kubernetes: https://rook.io/ 
    3. Integrate (How do i make my deployed storage available in my xluster)
        * Data services - your app must handle discovery and negotiation
        * Block/ File storage - Use a Container storage interface (CSI> driver
            - Without k8s: Swap requires ewrite your app to handle provisitoning, attaching, mounting
            - With k8s: Swap is as eeast as deploying a new CSI driver
    4. Consume (how do my stateful app provision and use available storage)
        * Data Servies - your app must handle it!
        * Block/ File storage - use K8s storage API (PVC, PV, etc)
            - Without k8s: Manual provision, manual attach, manual mount
            - With k8s: Automatic provisioning

-------

## Create Visually Compelling Developer Experiences for Kubernetes on VS Code - Ivan Towlson & Ralph Squillace

# Introductino
* Myth: All developers <3 the command line
* Reality: Different people, different experience
* Pitfalls of command line
    - Discoverability 
    - Visualisation 
    - Context awareness
* Good article to read about k8s principles: https://cloudblogs.microsoft.com/opensource/2019/05/21/kubecon-microsoft-updates-helm-3-virtual-kubelet-1-visual-studio-code-service-mesh-interface/ 


### Introducting the extension
* Available here (v1.0): https://marketplace.visualstudio.com/items?itemName=ms-kubernetes-tools.vscode-kubernetes-tools 
* Features
    - Context aware
    - Version aware
    - Tololchain aware
    - Task aware
* Openshift connector to VS Code 
* LInkerd extension
* Weave scope extension 

### Writing a VS code extension
* Run:
```
yo code
npm install
```
* Open folder in VS Code
* Add to package.json
```
extensionDependencies...
```
* Install )optional but makes it easier)
```
npm install vscode-kubernetes-tools-api
```
* In Package.json, update
```
activationEvents...
menues... view/item/context... 
```
* Run (using F5)

-----

## The Story of Why We Migrate to gRPC and How We Go About It - Matthias Grüter
* "Developers don't care about new RPC technologies" 

### Background
* Infra
    - 200M active
    - 8M QPS at ingress
    - 1k developers
    - 2.5 services
    - Java python
* Messaging layer: Services communicate primarily using our their protocol called Hermes
    - Details here: https://www.quora.com/What-is-Spotifys-architecture
* Hermes
    - built on 2012
    - Based on ZeroMQ
    - JSON or Protobuf
* Why replace Hermes?
    - Ecosystem doesn't exist
    - It is TCP based and that's about it!
    - No tooling (for debugging)
* From DIY to FOSS (Free Open Source Software)

### Why gRPC?
* Philosophy
    - Open
    - Support HTTP2
    - Backed by CNCF
    - Strongly typed=

### Schema Management
* The Proto
    - holy-grail
    - Code Generation in Java, Golang, Pythong, etc
    - Single source of truth
    - It's improtant to treat proto files as the first class citizenv
    - Tool by Uber for proto files: https://github.com/uber/prototool 
* Shared repo for all protos (this becomes your repository)
    - This is where you can have guidelines
    - Submit PRs
    - Ci pipeline 
* Version on the proto package. Eg:
```
package sportify.playlist.v1;
package.playlist.v2beta1;
```

### Features
1. Retries
2. Retry Throttling & Pushback
3. Deadlines (per request)
4. Load Balancing and proxying
    - Lookaside load balancing (https://grpc.io/blog/loadbalancing/) 
    - Lookaside LB is pretty much like control plane
5. Proxyless RPS mesh (coming up soon?)
    
### How?
* Autonmous Teams
* Aligned AUtonomy
    - Worth checking: https://www.slideshare.net/jchyip/enabling-autonomy-at-spotify 
* Carrot
    - Tracing
    - Code generation
    - cool tool x
* Stick

-------

## Diversity Hack

* Goal: To build k8s locally and contribute back
* Contributor guide: https://docs.google.com/presentation/d/1usEbwHMSC8vR7HvbxHJBOj2ISdTkw9rmufQUq7fkIl4/edit?usp=sharing 
* Join the slack channel: kubernetes-dev
* Run k8s locally (more details in the contirbutor guide):
```
git clone https://github.com/kubernetes/kubernetes
cd kubernetes
make WHAT=cmd/kubectl
_output/bin/kubectl version // check timestamp
```
* Great starting point: https://github.com/kubernetes/dashboard/labels/good%20first%20issue 
* Issue details
    - https://github.com/kubernetes/kubernetes/issues/68026 
    - Add details here: https://docs.google.com/spreadsheets/d/1VhU6zCk-vaAnbu_XkgsIq5I7jVnezUquWBMhcIN6AJM/edit#gid=0 
    - Issue that I picked up: staging/src/k8s.io/apiserver/pkg/features/kube_features.go

-------

## Service Mesh <3 legacy

### Container strategy
* K8s as centrally provided orchestration platform
    - Fast deployment cycles
    - Focus on soft mult-tenancu
    - Focus on microservices
* Multiple clusters decoupled on network dimensions
    - front-end, back-end infra, data center, live/non-live
    - Runs on bare-metal on-prem

### Legacy Gap
* Around 1000 existing services in prod
    - 90% on VMs and bare metal
    - 10% on Kubernetes
* Complex dependencies
* Not cloud-native ready
    - Not stateless
    - IP based ACLs

## Benefits of using Service Mesh
* Deal with the service mess of microservices
* Externalise required standard functionality 
    - Telemetry
    - Request routing
    - Circut breaking
    - Rate limiting
    - mTLS
    - AuthN, AuthZ
* Declarively configured 
* Centrally maintained
* Language agnostic

### General connectivity 
* In mesh
* Out of mesh
    - Ingress Gateway
    - Egress Gateway

### Connecting to the old world
* Mesh expansion (based on Istio 1.0.x)
* Install Envoy on hosts and connec to Istio control plane
* mTLS: Universal transport encryption and authentication
    - Unfiromly configured with K8s resourves
* Central observability by Istio telemetry

### Problems with Operating Istio
* Security concerns
    - High privileges for control place components
        * run as root
        * writing root filesystem
    - High privleges for admins and serviceaccounts (to use iptables)
        * net_admin capabilities
        * run as root
    - Same problem with book into sample application
* Sidecar injection
    - Problematic order of automatic sidecar injection vs PRP evaluation

### Problems with Mesh Expansion
* Connectivity to control place
* Telemetry / Policy
* Inbound (in-mesh) calls
* mTLS setup

### Service Mesh 'lite'
* Mimic mTLS with custom proxy setup and static certificates
* Automatic sidecar injection as part of deploymeny pipeline

### Istio 1.1 to the resue
* Good
    - Control plane connection
    - Outbound expansion
    - mTLS setup
    - Istio-CNI / Security concerns
* Bad 
    - Inbound expansion
    - Automatic sidecar injection
* Unsure
    - Documentation complex
    - Documentation partially inconsistent
    - Multi-tenancy unclear

-------

## Strategies to "Kubernetify" Legacy Applications - Sai Vennam

Note: 
* IBM electron app for visualising kubectl: https://github.com/IBM/kui 
* Quickly create dynamic end-to-end REST APIs: https://loopback.io/ 

1. Lift and Shift
* Pro: Speed to market
* Con: Technical Debt
* Strategy:
    - Containerize (or Dockerize it!) and Kubernetify 
    - Extract Config (Create yaml file)
    - Create Persistence for Stateful Apps
* Add £PICTURE£ from PDF

2. Innovate
* Pro: New end-user experiences
* Con: Legacy architecture restrictions
* Strategy:
    - Create new cloud-native apps
    - Build off existing workloads and apps
    - Identify bottle-necks
* Add £PICTURE£ from PDF

3. Deconstruct
* Pro: Performant modern apps
* Con: Time consuming
* Strategy:
    - Identify sercice to deconstruct
    - Create GLUE code (APIs) to modernize legacy apps
    - Refactor and elimate legacy apps
* Add £PICTURE£ from PDF


-------



