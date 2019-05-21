## Keynote

### Talk 1: Dan Kohn - Stitching things together
* 7700 attendees in KubeConEurope 2019
* Development of new technologies is based on availablity of other technologies
* Hance, facebook was required - this is simulateous invention
* In 1858, Charles Darwin & Alfred Russell Wallace both discovered 'evolation'. Another example of simultaeous invention
* 1687, Issac Newton
* Pre-requisite inventions have been done. Now, we need a spark for the next step
* 2010s, internet, linux, TCP-IP were ready - enabled web-scaled were created by multiple companies (eg: Mesos, Cloud Foundry, Borg, Uber Pelaton, Helios)
    > Everything is a remix
* Why k8s?
    - It works really well
    - Vendor neutral open source (CNCF was created in linux foundation to not fragment the ecosystem)
    - It's the people

### Talk 2: Cheryl Hung: 2.66 Million
* Director of Ecosystem
* Started in Google by writing C++ using Borg
* Power of Open Source in CNCF (Github only!)
    - 2.66 million contributions
    - 56,214 contributors
* Starting a new user group - CNCF Financial Services
* Slides: http://www.oicheryl.com/ 

### Talk 3: Bryan Liles: CNCF Project Update
* This morning's theme is "Pervasiveness of kubernetes"
* Program Committee - 107 members, 1535 subimissions!
* Context CNCF projects work
    - empower organisations to build and run scalable applications in modern dyamic environments such as public, private and hybrid clouds. Containers, service meshes, e
* Example book "Crossing the chasm" 
* Project types:
    - Sandbox (16)
    - Incubating (16)
    - Graduated (6)

* Sandbox
    - OpenEBS

* Incubating
    - Linderd (lightweight service mesh... without code changes)
        * Creator of linkerd
        * 10,000 stars, 2000 slack channel members, 100+ contributors
        * Kinvolk service mesh benchmark harnes
        * Automatic mTLS  - Linkerd 2.3 has zero-config proxy with mTLS
    - Helm (kubernetes package manager)
        * client-side only
        * Push charts to OCI resitries
        * Validate chart configuration with JSON schema
        * desclare dependencies in Chart.yaml instead requirements.yaml
        * tiller removal!
        * Validate chart values
        * https://v3.helm.sh/ 
    - Harbor (open source cloud native registry)
    - Rook (production ready file storage)
        * https://rook.io/docs/rook/v1.0/ceph-block.html
    - CRI-O (implementation of k8s CRI to enable the use of OCI compatible runtimes)
        * By Redhat
        * Example: Replace containerd with CRI-O
        * CRI-O - implements k8s container runtime interface (supporting OCI)
        * Built on top of building blocks following the unix philosophy
        * CRI-O only supports k8s
        * Goal: "Make running containers in production secure and boring"
        * Comparing with Docker: https://www.reddit.com/r/kubernetes/comments/9guswb/docker_vs_crio/
    - OpenCensus + OpenTracing
        * Observability - how well can you understand your system given only the telemetry data that makes it out
        * It must becoma built-in feature of cloud-native software
        * OpenTracing + OpenCensus = OpenTelemetry
        * Merging the technology and the communication: The next major version (and also maintaining the backward-compatibility)

* Graduated
    - FluentD
        * Logging is to solve data analysis
        * Applications are distributed 
        * Logs are streams of data
        * Data comes in multiple formats
        * To solve this problem.... unified logging layer!
        * Community has built near 100 plugins
        * Article: https://www.cncf.io/blog/2016/12/08/fluentd-cloud-native-logging/ 

### Talk 4: Vijay Pandey
* Evolution of networking in cloud native
* CISCO built boxes that are connected with protocol
* Protocols became complicated
* Simplification
    - Separate software from hardware
    - Simple algos in lieu of trafitional protocol stakcs
* The cheat: Next step for networks was to put it in one BIG cardboard box (vswitches, vnetworks, etc)
* Let's not cheat this time! Simplifity connectivity
* Introducting the Network service mesh
    - Hybrid and multicloud connectivity + security
    - without using IP, routers, switches, etc

### Talk 5: Lucas Käldström & Nikhita Raghunath: Getting Started in the Kubernetes Community
* 31,000+ unique k8s contributors till date
* Diversity lunch, mentor-mentee sessions\
* Have a 'community-first' mindset i/e Community over product
* "Culture eats strategy for breakfast" -- Peter Drucker
* Community Groups
    - Special Interest Groups
    - Working Groups
    - User Groups
    - Committees
* Great starting repo: https://github.com/kubernetes/community
    - Look for [good first issue](https://github.com/search?q=org%3Akubernetes+org%3Akubernetes-sigs+org%3Akubernetes-incubator+org%3Akubernetes-csi+org%3Akubernetes-client+is%3Aopen+is%3Aissue+label%3A%22good+first+issue%22&type=Issues)
* Slack channel: http://slack.k8s.io/
* All SIGs have
    - Public meetings
    - Summary on google docs
    - Recording to 
* Steps of being part of being contributions:
    - Joining slack and public meetings
    - KEP (k8s enhancement proposal)
    - Member 
    - Reviewer
    - Approver
    - Owner

------

## Hands-On: GitLab Serverless workshop using Knative

### Notes
* See  serverless-workshop.pdf
* CNCF ecosystem is getting very complicated
* Epic article: https://circleci.com/blog/its-the-future/ 
* Repo: https://gitlab.com/gitlab-workshops/serverless-workshop 
* It's complicated
    - Create a cluster
    - Install an app packager
    - Install 2-3 systems on top of service
    - Networking paradigm
    - Finally get your app running!
* Serverles:
    - Event driven architecture
    - Serviceful
    - Fine grained pay as you go
    - FaaS as processing between cloud services linked by events
* Knative as a Solution!
* Operator Frameworks
    - Kubebuilder
    - Operator Framework
    - Metacontroller 
* Serverless framework: https://serverless.com/framework/docs/getting-started/ 
* What's happening under the hood?
    - gitlab triggers the pipeline using the gitlab-si.yml
        ```yaml
            stages:
            - deploy-function

            deploy-hello-function:
            stage: deploy-function
            environment: test
            image: gcr.io/triggermesh/tm:latest
            before_script:
                - echo $TMCONFIG > tmconfig
            script:
        - tm --config ./tmconfig deploy --wait; echo
        ```
    - This uses serverless.yml to under
        ```yaml
        service: functions
        description: "Deploying functions from GitLab using Knative"

        provider:
        name: triggermesh

        functions:
        hello:
            source: lab1/hello
            runtime: https://gitlab.com/gitlab-workshops/workshop-resources/knative-lambda-runtime/raw/master/python-3.7/buildtemplate.yaml
            description: "python Hello function with KLR template"
            buildargs:
            - DIRECTORY=lab1/hello
            - HANDLER=hello.endpoint
        ``` 
------

## Hands-On: Deployment of Stateful Workloads on Kubernetes

### Agenda
* [Slides](stateful-workloads-on-k8s.pdf)
* Create a Cluster
* Basic Stateful Workload Concepts
* Dynamic Provisioning
* Higher Level Workload Concepts
* Kubectl
* Common Debugging Techniques
* Our Cassandra Demo App Hands-On
* Other databases
* Advanced Topics

### Steps:
1. Create a k8s cluster on GCP
2. Create a pod, PersistentVolumeClaim, PersistentVolume (see £PICTURE£)
3. Push it to GCP and see it work

### Demos worth checking out:
1. Cassandra demo: https://github.com/jsafrane/caas/ 
2. MySQL: https://kubernetes.io/docs/tasks/run-application/run-replicated-stateful-application/
3. PostgreSQL: https://github.com/CrunchyData/crunchy-containers 
4. Mongo: https://codelabs.developers.google.com/codelabs/cloud-mongodb-statefulset/index.html?index=..%2F..index#0  (** worth trying out **)

### Cloud-native databases
* CockroachDB
* FoundationDB
* TiDB
* Vitess
* YugaDB

------

## Intro to Linkerd

### What is Linkerd?
* Only Service Mesh in CNCF project
* 24+ months in production
* Very active slack channel
* Two flavours
    - Linkerd 1.x
    - Linkerd 2.x

### Why use Linkerd?
* Visibility - automatic golden metrics - success rates, latencies, throughput
* Reliability - retys, timeouts, cicuit breaking, deadlines, request balancing
* Security - Transparent mTLS, cert validation, policy
* Goal - move visibility, reliability, security primitives into the infrastructure layer, out of the applicatino layer

### Linkderd vs Istio?
* This is driven from the goals defined in linkerd (specially linkerd 2.x)
* Similar first principles, different design goals

### Linkerd 2.x architecture
* linkerd-proxy: which is the sidecar
* contoller: main control plane
* cli, web: to interact and view the service mesh
* telemetry: prometheus, grafana (this is temporary and so, you need to store it somewhere proper!)

### Articles worth reading:
* Linkerd Community Guide to KubeCon EU 2019: https://buoyant.io/2019/04/23/linkerd-community-guide-to-kubecon-eu-2019/
* Linkerd Benchmarks: https://linkerd.io/2019/05/18/linkerd-benchmarks/ 

------

## Hands-On: A day in the life of a cloud native application developer

Note: This workshop was full and so, details can be found [here](cloud-native-workshop.pdf)

------

## Laying the Foundation: Real World Kubernetes Deployment Patterns 

### Landing Zone 
* (where will you deploy your cluster)
* Managed k8s
    - Eg: AKS, EKS, GKE
    - What if you want access to the control plane?
* Distribution
    - Eg: Openshift      
* Roll your own
    - Need deep insights
    - There are certain things that you need to know

### Landing Zone: Missteps
* All-in with managed k8s
    - Very ideal, not a panacea
    - Costs associated with someone else managing your control plane
* One-size fits all installers
    - INfra is almost always snowflakey
    - Are you comfortable with supporting this?

### Principles
* Yesterday, today and tomorrow
    - Eg: Ingress controller can be nginx today, something else tomorrow
    - Check out: https://itnext.io/kubernetes-ingress-controllers-how-to-choose-the-right-one-part-1-41d3554978d2 
* Back to the basics
* Enable your stakeholders
    - Give log and monitoring access 

### Principles: Missteps
* Solving problems you don't yet have
    - New solutions appearing daily
    - More solutions... more porblems
    - eg: Service Mesh... conceptually, it makes sense! But do you have that problem today?
* Perfect is the enemy of done
    - Wasting cycles "guessing" about requirements
    - Time could be used gaining operational experience
    - In other words, narrow your scope!

### Bedrock
* (there are some thing that you won't change)
* Container Networking
* Persistent Storage
* Connectivity 

### Bedrock: Missteps
* Trading "battel-tested" for "cutting-edge"
    - Slow down a little
* open source is not free... it requires diligence
    - Community health
    * Release cycles
    * Github stars (most important)

### Security 
* (over obscurity)
* Multi-team? Multi-tenant?
* Consistent AuthZ / AuthN
* Policies
* Backup and restore

### Security: Missteps
* One cluster to rule them all
    - Hard isolation is hard
* Not getting security involved early
    - This journey is for everyone 
* Adding security "later"
    - Not easy to bolt-on

### Scale out
* (but you are likely not Google, Amazon or MSFT)
* Multi-cluster mindset
* That 'F' word: Federation
* Resources

### Scale out: Missteps
* Scaling out MEGA clusters
    - Hard to manage over time
    - Putting all your eggs in one basket
    -consider smaller clusters you can stamp out with ease

### Embrace
* (and extend)
* Highly extensible
* Plan on extending
* Operators (worth reading: https://github.com/kubeflow/tf-operator/issues/300)

### Embrace: Missteps
* Extending k8s non-natively
    - Ignoring core k8s concepts
    - Building out fragile flows

------

## Birds of a Feather: Financial Services User Group - Cheryl Hung, CNCF

This is an informal session where different financial services individuals about there concerns

### Get involved:
* https://github.com/cncf/financial-user-group 
* financial-user-group@lists.cncf.io
* Monthly calls on 4th Tuesday, 14:00 UTC
* chung@linuxfoundation.org 

------

## Keynote

### Talk 1: Welcome Remarks - Janet Kuo

### Talk 2: Democratising Service Mesh on Kubernetes - Gabe Monroy
* Smart endpoints, dumb pipes - this has worked for the past 25 years
* Smarter pipes 
    - Instio
    - Consul (HashiCorp)
    - Linkerd - lightweight
* Introducing Service Mesh Interface (SMI)
    - Policy (how to make it easy e.g mTLS)
    - Telemetry (how to capture key metrics)
    - Routing (shift and weight traffic)
* Note Ingress does not implment, it is only an interface which is implemented by
    - Nginx
    - Trafik
    - HaProxy
* Similarly, SMI is only an interface that needs to be implements
* Why SMI
    - Get started quickly
    - Simpler is better
    - Ecosystem friendly

### Talk 3: K8s Project Update - Janet Kuo
* History
    - 2003: Borg (Predecessor of k8s)
    - 2006: Process Containers (cgroups - Linux Control Groups)
    - 2008: Containers
    - 2009: Omega (next-gen Borg)
    - 2013: Docker open-source, Project 7 (open source container orchestrator)
    - 2014: Kubernetes announced at DockerCon
    - 2015: CNCF & 1st KubeCon & Kubernetes 1.0
    - 2016: SIG (System Interest Group) & KubeCon EU & Industry Aoption in Prod
    - 2017: De Facto Standard (Native Kubernetes support), introduced features like CRDs, RBAC 
    - 2018: Graduation from CNCF & First KubeCon in Asia
    - Today: #2 PRs across GitHub, #4 Issues & authors
* What's in 1.14?
    - Windows nodes GA (https://kubernetes.io/blog/2019/03/25/kubernetes-1-14-release-announcement/)
    - Local PV GA
* Future
    - Extensibility: More platforms & frameworks, CRD still beta
    - Scalability: Not a simle number, not independent, bottlenecks
        * Node Status is expensive. Node Status: 300-600MB/min for a 5000 nodes cluster. This can break etcd. This needs to be solved.
    - Reliability: Cascading failures. Bad pods kill nodes. Eventually kill the cluster
        * Workaround is to hide signal. This needs to be solved.

### Talk 4: Recursive Kubernetes: Cluster API and Clusters as Cattle - Joe Beda
* kube-up.sh alternatives
    - https://stackoverflow.com/questions/41180673/kube-up-sh-is-deprecated-what-is-the-alternative 
    - Bootstrapping: kubeadm (assumption is that you have some computers and you can bootstrap a k8s cluster)
    - Provisioning: cluster API (using CRDs to extend kubernetes to manage machines)
        * 'Cluster'
        * 'Machine' - whic is under 'Cluster'
        * also, 'MachineSet' and 'MachineDeployment
    - SIG Cluster Lifecycle

### Talk 5: Reperforming a Nobel Prize Discovery on Kubernetes - Ricardo Rocha & Lukas Heinrich
* CERN
    - Fundamental research - about physics (eg: antimatter research)
    - Creation of web
* Stack
    * GCP Cluster on GKE
    * Job Results
    * Jupyter for interactive visualisation
* Re-created the higgs boson particle detection simulation

### Talk 6: Expanding the Kubernetes Operator Community - Rob Szumski
* K8s adoption phases
    1. Stateless app
    2. Stateful apps
    3. Dsitributed apps
* Operators run your complex apps
    - Embed ops knowledge from the experts
    - Operator Framework: https://github.com/operator-framework 
* Options to run postGreSQL
    - Docker
    - Cloud Database
    - Crunchy data (http://guincodes.blogspot.com/2018/06/using-crunchy-data-postgresql-operator.html) 

