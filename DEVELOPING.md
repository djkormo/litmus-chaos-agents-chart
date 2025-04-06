Testing locally

```
helm template ./charts/litmus-chaos-agents/  --namespace litmus-system --values ./charts/litmus-chaos-agents/values.yaml
```

```
helm template ./charts/litmus-chaos-agents/  --namespace litmus-system  --values  ./charts/litmus-chaos-agents/values.yaml | grep "image:"
```

```
helm template ./charts/litmus-chaos-agents/  --namespace litmus-system  --values ./charts/litmus-chaos-agents/values.yaml | grep "namespace:"
```

```
helm template ./charts/litmus-chaos-agents/  --namespace litmus-system  --values  ./charts/litmus-chaos-agents/values.yaml > all-litmus-chaos-agents.yaml
```

```
kubectl apply -f all-litmus-chaos-agents.yaml -n litmus-system --dry-run=client
kubectl apply -f all-litmus-chaos-agents.yaml -n litmus-system --dry-run=server
```

Example of values.yaml file



```yaml
# Whether to install Custom Resource Definitions (CRDs).
crd:
  install: false
# Whether to install RBAC resources (ServiceAccount, ClusterRole, ClusterRoleBinding).  
rbac:
  install: false
  serviceAccountName: litmus # Name of the ServiceAccount to be used by the agent.
  serviceAccountAdminName: litmus-admin # Name of the ServiceAccount to be used by the admin agent.
  serviceAccountChaosName: argo-chaos # Name of the ServiceAccount to be used by the admin agent.

configuration:
  scope: cluster # Scope of the installation. Can be either 'cluster' or 'namespace'. If set to 'namespace', the namespace should be specified in the 'namespace' field.
  instanceID: REDACTED # Unique ID of the Litmus instance.
  accessKey: F9boAe-REDACTED # Access key for the Litmus instance.
  serverAddress: http://localhost:31674/api/query # Address of the Litmus Portal server.
  infraScope: cluster # Can be either 'cluster' or 'namespaced'. This is used to determine the scope of the infrastructure resources.
  version: 3.16.0
  startTime: ""
  skipSSLVerify: "false"
  customTLSCertificate: ""
  isInfrastructureConfirmed: "false"
  componsnts: |
    DEPLOYMENTS: ["app=chaos-exporter", "name=chaos-operator", "app=event-tracker", "app=workflow-controller"]

chaosSubscriber: 
  enabled: true # Whether to install the subscriber agent.
  image: litmuschaos.docker.scarf.sh/litmuschaos/litmusportal-subscriber:3.16.0 # Image of the subscriber agent.

chaosExporter:
  enabled: true # Whether to install the chaos exporter agent.
  image: litmuschaos.docker.scarf.sh/litmuschaos/chaos-exporter:3.16.0 # Image of the chaos exporter agent.  

chaosOperator:
  enabled: true # Whether to install the chaos operator agent.
  image: litmuschaos.docker.scarf.sh/litmuschaos/chaos-operator:3.16.0 # Image of the chaos operator agent.  

chaosEventTracker:
  enabled: true # Whether to install the chaos event tracker agent.
  image: litmuschaos.docker.scarf.sh/litmuschaos/litmusportal-event-tracker:3.16.0 # Image of the chaos event tracker agent.

chaosWorkflowController:
  enabled: true # Whether to install the chaos workflow controller agent.
  image: litmuschaos.docker.scarf.sh/litmuschaos/chaos-workflow-controller:v3.3.1 # Image of the chaos workflow controller agent.
  imageArgoExec: litmuschaos.docker.scarf.sh/litmuschaos/argoexec:v3.3.1 # Image of the Argo .


```