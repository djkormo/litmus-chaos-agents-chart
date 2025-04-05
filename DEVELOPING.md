

helm template ./charts/litmus-chaos-agents/  --namespace litmus-system --values ./charts/litmus-chaos-agents/values.yaml

helm template ./charts/litmus-chaos-agents/  --namespace litmus-system  --values  ./charts/litmus-chaos-agents/values.yaml | grep "image:"

helm template ./charts/litmus-chaos-agents/  --namespace litmus-system  --values ./charts/litmus-chaos-agents/values.yaml | grep "namespace:"

helm template ./charts/litmus-chaos-agents/  --namespace litmus-system  --values  ./charts/litmus-chaos-agents/values.yaml > all-litmus-chaos-agents.yaml



kubectl apply -f all-litmus-chaos-agents.yaml -n litmus-system --dry-run=client
kubectl apply -f all-litmus-chaos-agents.yaml -n litmus-system --dry-run=server