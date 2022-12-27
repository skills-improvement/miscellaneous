# Docker Desktop Configuration

After installing Docker Desktop,

1. Configure some basic settings,
  - Resources -> Advanced -> CPUs -> "6"
  - Resources -> Advanced -> Memory -> 12GB
  - Resources -> Advanced -> Swap -> 2GB
  - Resources -> Advanced -> Virtual Disk Limit -> 150GB
  - Kubernetes -> Enable Kubernetes
  - Kubernetes -> Show system containers
  - Software Updates -> Check for Updates (27/12/2022 - 4.15.0 DD, 1.25.2 K8s)
2. Setup the Metrics Server, [SO Ref](https://stackoverflow.com/questions/54106725/docker-kubernetes-mac-autoscaler-unable-to-find-metrics)
  - Clone the Metrics Server: git clone https://github.com/kubernetes-sigs/metrics-server.git
  - Update the `components.yaml`; add `--kubelet-insecure-tls` to `template:` -> `spec:` -> `containers:` -> `args`
  - Create the Metrics Server, `kubectl apply -f components.yaml`
  - Verify `metrics-server` is running, `kubectl get pods --namespace kube-system | grep metrics-server`
  - Wait for about 2-3 minutes then test it; `kubectl top pods`, `kubectl top nodes`
3. Create some base Namespaces for K8s
  - `kubectl create namespace <development/staging/production/gitlab/argocd/...>`
  - Source control a set of Namespace templates
4. Create a set of basic administrative Dockerfiles for Ansible and other tooling
  - Simple Dockerfile with VIM and other tools
  - Simple Dockerfile with above plus Ansible
