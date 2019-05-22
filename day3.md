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