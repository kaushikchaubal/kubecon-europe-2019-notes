
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