# Kube Lab Answers

## Pod

```bash
kubectl run lab-2048 --image=ghcr.io/cis1912/2048 --port=80
```

`kubectl get pods` showed `lab-2048` as `1/1 Running`. `kubectl describe pod lab-2048` showed the label `run=lab-2048` and the events `Scheduled -> Pulling -> Pulled -> Created -> Started`.

`kubectl port-forward pod/lab-2048 8080:80` served the 2048 page on `localhost:8080`.

## Service

Command used to expose the pod:

```bash
kubectl expose pod lab-2048 --port=80 --target-port=80
```

This created a `ClusterIP` service named `lab-2048` with selector `run=lab-2048` (copied from the pod's label), port 80 -> targetPort 80. Its endpoint was the pod's IP on port 80.

`kubectl port-forward svc/lab-2048 8080:80` also served the 2048 page.

## Pod vs. service port-forward

Forwarding to a pod tunnels to that one specific pod. Forwarding to a service resolves the service's selector to a backing pod and tunnels there, so the command keeps working even if the pod is replaced (as long as a new pod has the matching label). Applications normally talk to each other through services for that reason.
