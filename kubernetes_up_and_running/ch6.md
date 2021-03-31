# Chapter 6 - Labels and Annotations

Labels - key/value pairs that can be attached to Kubernetes objects such as Pods and ReplicaSets. Used for grouping

Annotations - provide a storage mechanism that resembles labels: annotations are key/value pairs designed to hold nonidentifying information that can be leveraged by tools and libraries

## Labels

Label keys can be broken down into two parts: an optional prefix and a name, separated by a slash

The prefix, if specified, must be a DNS subdomain with a 253-character limit

The key name is required and must be shorter than 63 characters

Names must also start and end with an alphanumeric character and permit the use of dashes (-), underscores (_), and dots (.) between characters

Examples:

`acme.com/app-version   1.0.0`

`appVersion   1.0.0`

`app.version   1.0.0`

`kubernetes.io/cluster-service true`

Applying labels examples:

```
kubectl run alpaca-prod \
  --image=gcr.io/kuar-demo/kuard-amd64:blue \
  --replicas=2 \
  --labels="ver=1,app=alpaca,env=prod"
```

```
kubectl run alpaca-test \
  --image=gcr.io/kuar-demo/kuard-amd64:green \
  --replicas=1 \
  --labels="ver=2,app=alpaca,env=test"
```

```
kubectl run bandicoot-prod \
  --image=gcr.io/kuar-demo/kuard-amd64:green \
  --replicas=2 \
  --labels="ver=2,app=bandicoot,env=prod"
kubectl run bandicoot-staging \
  --image=gcr.io/kuar-demo/kuard-amd64:green \
  --replicas=1 \
  --labels="ver=2,app=bandicoot,env=staging"
```

Result:

```
kubectl get deployments --show-labels

NAME                ... LABELS
alpaca-prod         ... app=alpaca,env=prod,ver=1
alpaca-test         ... app=alpaca,env=test,ver=2
bandicoot-prod      ... app=bandicoot,env=prod,ver=2
bandicoot-staging   ... app=bandicoot,env=staging,ver=2
```

### Modifying Labels

`kubectl label deployments alpaca-test "canary=true"`

You can also use the -L option to kubectl get to show a label value as a column:

```
kubectl get deployments -L canary

NAME                DESIRED   CURRENT   ... CANARY
alpaca-prod         2         2         ... <none>
alpaca-test         1         1         ... true
bandicoot-prod      2         2         ... <none>
bandicoot-staging   1         1         ... <none>
```

You can remove a label by applying a dash suffix:

`kubectl label deployments alpaca-test "canary-"`

### Label Selectors

Label selectors are used to filter Kubernetes objects based on a set of labels. Selectors use a simple Boolean language. They are used both by end users (via tools like kubectl) and by different types of objects (such as how a ReplicaSet relates to its Pods)

Each deployment (via a ReplicaSet) creates a set of Pods using the labels specified in the template embedded in the deployment. This is configured by the kubectl run command

Running the kubectl get pods command should return all the Pods currently running in the cluster

Example:

```
kubectl get pods --show-labels

NAME                              ... LABELS
alpaca-prod-3408831585-4nzfb      ... app=alpaca,env=prod,ver=1,...
alpaca-prod-3408831585-kga0a      ... app=alpaca,env=prod,ver=1,...
alpaca-test-1004512375-3r1m5      ... app=alpaca,env=test,ver=2,...
bandicoot-prod-373860099-0t1gp    ... app=bandicoot,env=prod,ver=2,...
bandicoot-prod-373860099-k2wcf    ... app=bandicoot,env=prod,ver=2,...
bandicoot-staging-1839769971-3ndv ... app=bandicoot,env=staging,ver=2,...
```

The `--selector` flag can be used to show pods that match certain criteria:

```
kubectl get pods --selector="ver=2"

NAME                                 READY     STATUS    RESTARTS   AGE
alpaca-test-1004512375-3r1m5         1/1       Running   0          3m
bandicoot-prod-373860099-0t1gp       1/1       Running   0          3m
bandicoot-prod-373860099-k2wcf       1/1       Running   0          3m
bandicoot-staging-1839769971-3ndv5   1/1       Running   0          3m
```

If we specify two selectors separated by a comma, only the objects that satisfy both will be returned. This is a logical AND operation:

```
kubectl get pods --selector="app=bandicoot,ver=2"

NAME                                 READY     STATUS    RESTARTS   AGE
bandicoot-prod-373860099-0t1gp       1/1       Running   0          4m
bandicoot-prod-373860099-k2wcf       1/1       Running   0          4m
bandicoot-staging-1839769971-3ndv5   1/1       Running   0          4m
```

We can also ask if a label is one of a set of values. Here we ask for all Pods where the app label is set to alpaca or bandicoot (which will be all six Pods):

```
kubectl get pods --selector="app in (alpaca,bandicoot)"

NAME                                 READY     STATUS    RESTARTS   AGE
alpaca-prod-3408831585-4nzfb         1/1       Running   0          6m
alpaca-prod-3408831585-kga0a         1/1       Running   0          6m
alpaca-test-1004512375-3r1m5         1/1       Running   0          6m
bandicoot-prod-373860099-0t1gp       1/1       Running   0          6m
bandicoot-prod-373860099-k2wcf       1/1       Running   0          6m
bandicoot-staging-1839769971-3ndv5   1/1       Running   0          6m
```

Finally, we can ask if a label is set at all. Here we are asking for all of the deployments with the canary label set to anything:

```
kubectl get deployments --selector="canary"

NAME          DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
alpaca-test   1         1         1            1           7m
```

### Selector Operators

| Operation | Description |
|---|---|
| `key=value` | `key is set to value` |
|`key!=value` | `key is not set to value`|
|`key in (value1, value2)`|`key is one of value1 or value2`|
|`key notin (value1, value2)`|`key is not one of value1 or value2`|
|`key`|`key is set`|
|`!key`|`key is not set`|

Examples:

`kubectl get deployments --selector='!canary'`

`kubectl get pods -l 'ver=2,!canary'`

### Label Selectors in API Objects

When a Kubernetes object refers to a set of other Kubernetes objects, a label selector is used. Instead of a simple string as described in the previous section, we use a parsed structure

For historical reasons (Kubernetes doesn’t break API compatibility!), there are two forms. Most objects support a newer, more powerful set of selector operators

A selector of `app=alpaca,ver in (1, 2)` would be converted to this:

```
selector:
  matchLabels:
    app: alpaca
  matchExpressions:
    - {key: ver, operator: In, values: [1, 2]}
```

All of the terms are evaluated as a logical AND. The only way to represent the != operator is to convert it to a NotIn expression with a single value

### Labels in the Kubernetes Architecture

Kubernetes is a purposefully decoupled system. There is no hierarchy and all components operate independently. However, in many cases objects need to relate to one another, and these relationships are defined by labels and label selectors

For example, ReplicaSets, which create and maintain multiple replicas of a Pod, find the Pods that they are managing via a selector. Likewise, a service load balancer finds the Pods it should bring traffic to via a selector query. When a Pod is created, it can use a node selector to identify a particular set of nodes that it can be scheduled onto. When people want to restrict network traffic in their cluster, they use NetworkPolicy in conjunction with specific labels to identify Pods that should or should not be allowed to communicate with each other. Labels are a powerful and ubiquitous glue that holds a Kubernetes application together. Though your application will likely start out with a simple set of labels and queries, you should expect it to grow in size and sophistication with time

## Annotations

Annotations provide a place to store additional metadata for Kubernetes objects with the sole purpose of assisting tools and libraries

While labels are used to identify and group objects, annotations are used to provide extra information about where an object came from, how to use it, or policy around that object. There is overlap, and it is a matter of taste as to when to use an annotation or a label. When in doubt, add information to an object as an annotation and promote it to a label if you find yourself wanting to use it in a selector

Annotations are used to:

- Keep track of a “reason” for the latest update to an object.

- Communicate a specialized scheduling policy to a specialized scheduler.

- Extend data about the last tool to update the resource and how it was updated (used for detecting changes by other tools and doing a smart merge).

- Attach build, release, or image information that isn’t appropriate for labels (may include a Git hash, timestamp, PR number, etc.).

- Enable the Deployment object (Chapter 10) to keep track of ReplicaSets that it is managing for rollouts.

- Provide extra data to enhance the visual quality or usability of a UI. For example, objects could include a link to an icon (or a base64-encoded version of an icon).

- Prototype alpha functionality in Kubernetes (instead of creating a first-class API field, the parameters for that functionality are encoded in an annotation).

Annotations are used in various places in Kubernetes, with the primary use case being rolling deployments. During rolling deployments, annotations are used to track rollout status and provide the necessary information required to roll back a deployment to a previous state.

Users should avoid using the Kubernetes API server as a general-purpose database. Annotations are good for small bits of data that are highly associated with a specific resource. If you want to store data in Kubernetes but you don’t have an obvious object to associate it with, consider storing that data in some other, more appropriate database.

### Defining Annotations

Annotation keys use the same format as label keys. However, because they are often used to communicate information between tools, the “namespace” part of the key is more important

The value component of an annotation is a free-form string field. While this allows maximum flexibility as users can store arbitrary data, because this is arbitrary text, there is no validation of any format. For example, it is not uncommon for a JSON document to be encoded as a string and stored in an annotation. It is important to note that the Kubernetes server has no knowledge of the required format of annotation values. If annotations are used to pass or store data, there is no guarantee the data is valid. This can make tracking down errors more difficult

Annotations are defined in the common `metadata` section in every Kubernetes object:

```
...
metadata:
  annotations:
    example.com/icon-url: "https://example.com/icon.png"
...
```

## Cleanup

`kubectl delete deployments --all`

If you want to be more selective, you can use the `--selector` flag to choose which deployments to delete

## Summary

Labels are used to identify and optionally group objects in a Kubernetes cluster. Labels are also used in selector queries to provide flexible runtime grouping of objects such as Pods.

Annotations provide object-scoped key/value storage of metadata that can be used by automation tooling and client libraries. Annotations can also be used to hold configuration data for external tools such as third-party schedulers and monitoring tools.

Labels and annotations are vital to understanding how key components in a Kubernetes cluster work together to ensure the desired cluster state. Using labels and annotations properly unlocks the true power of Kubernetes’s flexibility and provides the starting point for building automation tools and deployment workflows.
