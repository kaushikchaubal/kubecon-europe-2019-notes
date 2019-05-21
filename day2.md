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

