
## Mentor-Mentee Session

### Session 1
* Starting point: https://github.com/kubernetes/community
* Slack channel: http://slack.k8s.io 
* Steps to start getting more involved:
    - Solve issues on https://github.com/kubernetes/kubernetes/issues 
    - Join SIG from the list on https://github.com/kubernetes/community/blob/master/sig-list.md 
* Other points:
    - Working group (it's a focus group for a specific task with a deliverable attached to it. Short period of time)
    - User groups (CNCF)
    - CNCF ambassador (Bob)

### Session 2
* https://github.com/kubernetes-sigs 
* https://github.com/kelseyhightower/kubernetes-the-hard-way 
* https://kubernetes.io/blog/2017/11/kubernetes-easy-way/ 
* https://www.katacoda.com/courses/kubernetes 

### Session 3
* https://github.com/helm/charts 
* Cloud Foundry (PaaS) vs K8s - 
* Helm is a templating engine
* https://kubernetes.io/docs/setup/independent/create-cluster-kubeadm/ 
* Steps:
    - Dockerise 
    - Make it stateless (look at https://12factor.net/)
    - Try to run it k8s
    - services and/or ingress
* Areas for further research
    - Argo Workflow
    - Argo Events (https://argoproj.github.io/argo-events/) 
    - Kubeflow (https://www.kubeflow.org/docs/started/getting-started/)
    - Knative 
        * https://github.com/meteatamel/knative-tutorial
        * https://speakerdeck.com/meteatamel/serverless-with-knative
    - Tekton
    - Kubeadm
        * https://kubernetes.io/docs/setup/independent/create-cluster-kubeadm/ 

------

## Keynote
* Theme: Cloud Native / k8s is a journey and not a destination

### Talk 1: Kubernetes - Don't Stop Believin' – Bryan Liles
* k8s history: 31k contributors, 164k commits, 1.2 million comments
* What does optimal developer experience look like with kubernetes?
* CNCF Ecosystem has some themes missing
    - k8s in more places 
        * Running k8s on windows desktop?
        * k8s in a car?
        * k8s in a store? (similar to https://k3s.io/)
    - Go is a fine language but...
        * what about JavaScript, Java and other options?
    - Custom Resourvce Definitions
        * Operators pattern (creation of CRDs by implementing)
        * Rethinking your APIs
    - Build on vs Build With
        * PostGres being 'build with'
        * Kubernetes IN Docker - local clusters for testing Kubernetes https://kind.sigs.k8s.io/ 
    - Config Management

### Talk 2: Keynote: From COBOL to Kubernetes: A 250 Year Old Bank's Cloud-Native Journey - Laura Rehorst & Mike Ryan
* Lessons learnt during transitioning to k8s
* Stack
    - Application
        * App definitaion: Helm
        * CI/CD: CloudBees, Jenkins, Azure DevOps
    - Orchestration
        * Amazon EKS
        * Azure AKS
    - Runtime
        * Persistent Storage: TBD (will be CSI compliant)
        * Container runtime: Docker
        * Network: CNI
    - Provisioning
        * Automation & Config: Terraform
        * Docker Registry: Sonatype Nexus
    - Infra
        * AWS
        * Azure
    - Scanning
        * Twistlock
    - Secrets Management
        * Vault
    - Monitoring and logging
        * Splunk
        * Prometheus
* Add £PICTURE£ of Standard pipeline used by ABN Amro

### Talk 3: Metrics, Logs & Traces; What Does the Future Hold for Observability? - Tom Wilkie  & Frederic Branczyk
* Three pillars:
    - Metrics (timeseries data, latency)
        * OpenMetrics
        * Prometheus 
    - Logs / Events (certain events)
        * FluentD
    - Traces 
        * OpenTracing (OpenTelemetry)
        * Jaeger
* Three predictions
    - More correlation between pillars
        * Grafana loki - uses Prometheus's discovery to get log-stream
        * Connecting Elastic Search + Zipkin (open-tracing)
        * OpenCensus (openTelemetry)
    - New Signals and New Analysis
        * Forth pillar? Continuous profiling of Go programs
            - https://medium.com/google-cloud/continuous-profiling-of-go-programs-96d4416af77b 
    - Rise of index-free logs
        * Ok log: https://github.com/oklog/oklog
        * kubctl logs
        * Grafana loki: https://github.com/grafana/loki (Like Prometheus, but for logs)

------

## Testing your K8s apps with KIND - Benjamin Elder & James Munnelly
* Big focus on extensions and controllers

### Unit Tests
* Typically make use of the fake clitn
* In-memory implementation of apiserver/client
* Caveats
    - Great for simple things, but lots of ca
    - Race and timing issues not surfaced
    - No other contollers running

### Integration tests
* Kubebuilder / controller-runtime use this approach a lot
* Run etcd + apiserver (optionally controller-manager)
* Why?
    - Admision Control
    - Timing issues

### End to end tests
* Start a full kubernetes cluster, run the application and codify expected results
* Gives us the ultimate flexibility
* Black box testing (we don't assume implementation) - performance test
* But this can be slow and/or expensive
* Most PRs on k8s take about 1 hour long

### Why end-to-end tests?
* Certain edge cates that are only picked up in a real environment
* Kubernetes has a lot of controller, the way these inter-op is really important
* 'Gihting' can cause massive issues for a level based system

### KIND
* Users Docker containers to simulate nodes
* Fast and lightweight
* Steps:
    1. Boot cluster
        ```
        kind create cluster
        ```
    2. Build yor application
    3. Sideload app images (optional)
        ```
        kind load docker-image myapp:latest
        KUBECONFIG = $(king get kbeconfig)
        ```
    4. Run tests!

### Running in CI
* Kind can be used on many different CI platforms
* CircleCI and Travis currently documented

### Other ways to run kind
* kind as a library - no more bash!

### Container runtimes (OCI (Open Container Initiative))
* Docker
* Rkt (https://coreos.com/rkt/) 
* ContainerD (https://containerd.io/)
* Worth reading: https://medium.com/cri-o/container-runtimes-clarity-342b62172dc3 

------

## Repeatable Deployments with Kubernetes, Helm & Bazel - Rohan Singh

### Bazel
* Overview: https://docs.bazel.build/versions/master/bazel-overview.html 
* Getting Started: https://docs.bazel.build/versions/master/getting-started.html
* Java Tutorial: https://docs.bazel.build/versions/master/tutorial/java.html 

### How is this different from using Docker build?
* Determinism
* Instead of writing a docker file, you will write a bazel build file
* You don't need to have a docker image running
* You can use the output of one target to the input of another target
* You an build and deploy to multiple kube clusters

### Bazel Rules
* A rule defines a series of actions that Bazel should perform on inputs to get a set of outputs
* container_image
* java_library
* nodejs_binary
* sh_test
* They are primarily for advanced users (https://docs.bazel.build/versions/master/skylark/rules.html) 
* Local Kubernetes development with no stress. Checkout: https://github.com/windmilleng/tilt

------

## Let's Try Every CRI Runtime Available for Kubernetes. No, Really! - Phil Estes

### Background: OCI
* OCI (open container interface)
* Container runtimes: docker, containerd, cri-o, kata, firecrasker, gVisor, Nabla, singularity
* Registries: DockerHub, OSS distribution process, Qya

### K8s & CRI responsibilities
* K8s
    - K8s
* CRI
    - Pod conainer lifecycle
    - Image management
    - Status
    - Container lifesyslt

### What we plan to use?
* Docker
* containerd
* Firecracker
* Kata
* gVisor
* cri-o

Note: Useful command
```
kubectl get no -o wide
```

-------