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
* Diversity lunch, mentor-mentee sessions