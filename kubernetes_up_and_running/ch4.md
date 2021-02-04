# Chapter 4 - Common kubectl Commands

### Namespace

Namespaces - organizes objects in the cluster

`default` is default namespace

You can pass a different namespace in as a flag: `kubectl --namespace`

Pass `--all-namespaces` flag to list all Pods in cluster

### Context

Context - change default namespace more permanently

Stored in `kubectl` config, usually `$HOME/.kube/config`

Set default:

`kubectl config set-context my-context --namespace=mystuff`

Use new context:

`kubectl config use-context my-context`

## Viewing Kubernetes API Objects

View object:

`kubectl get <resource-name> <obj-name>`

Use JSONPath to extract certain fields:

`kubectl get pods my-pod -o jsonpath --template={.status.podIP}`

Get more detailed info about an object:

`kubectl describe <resource-name> <obj-name>`

## Creating, Updating, and Destroying Kubernetes Objects

`kubectl apply -f obj.yaml`

Make interactive edits in local file:

`kubectl edit <resource-name> <obj-name>`

View config history records:

`kubectl apply -f myobj.yaml view-last-applied`

Delete an object:

`kubectl delete -f obj.yaml`

`kubectl delete <resource-name> <obj-name>`

## Labeling and Annotating Objects

Add label:

`kubectl label pods bar color=red`

Remove label:

`kubectl label pods bar color-`

## Debugging Commands

See logs for running container:

`kubectl logs <pod-name>`

Execute a command in a running container:

`kubectl exec -it <pod-name> -- bash`

Attach to running process:

`kubectl attach -it <pod-name>`

Copy files to and from a container:

`kubectl cp <pod-name>:</path/to/remote/file> </path/to/local/file>`

Port forward:

`kubectl port-forward <pod-name> 8080:80`

List resources:

`kubectl top nodes`

`kubectl top pods`

## Command Autocompletion

```
# macOS
brew install bash-completion

# CentOS/Red Hat
yum install bash-completion

# Debian/Ubuntu
apt-get install bash-completion
```

Help:

`kubectl help`

`kubectl help <command-name>`