# Pattern Code: SSKBNTDSN01

## Pattern Name: Side Car Pattern

## Problem

Containers should have only one responsibility. But what about the use case where you have a web server with a log processor? 

## Explanation of the Pattern

SideCar design pattern is a form of Single-node, multiple containers application patterns.

SideCar Pattern advocates the usage of additional container for extending or enhancing the main container.This container can be called as sidecar pattern.

![Side Car Pattern](https://dmd0822.files.wordpress.com/2018/03/031318_1606_thesidecarp2.jpg?w=736)

Pic Credits: blog.davemdavis.net

## Implementation

![Side Car Pattern](https://vitalflux.com/wp-content/uploads/2017/12/Screen-Shot-2017-12-09-at-10.11.23-AM.png)

## Configuration

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: blog
spec:
  replicas: 1
  selector:
    matchLabels:
      name: blog
  template:
    metadata:
      labels:
        name: blog
    spec:
      containers:
      - name: git-sync
        image: k8s.gcr.io/git-sync:v3.0.1
        volumeMounts:
        - name: markdown
          mountPath: /tmp/git
        env:
        - name: GIT_SYNC_REPO
          value: https://github.com/kubernetes/git-sync.git        
        - name: GIT_SYNC_DEST
          value: git-sync
      - name: hugo
        image: k8s.gcr.io/hugo
        volumeMounts:
        - name: markdown
          mountPath: /src
        - name: html
          mountPath: /dest
        env:
        - name: HUGO_SRC
          value: /src/git-sync/demo/blog
        - name: HUGO_BUILD_DRAFT
          value: "true"
        - name: HUGO_BASE_URL
          value: example.com
      - name: nginx
        image: nginx
        volumeMounts:
        - name: html
          mountPath: /usr/share/nginx/html
      volumes:
      - name: markdown
        emptyDir: {}
      - name: html
        emptyDir: {}
```

## Benefits

**Separation of concerns** 

Let each container take care of their core concern. Web server container would be serving web pages while sidecar container would be processing server logs. This would be helpful during issues resolution stages when issues related to one concern should not conflict with other concern.

**Single responsibility principle**

 A container has primarily got one reason for change. In other words, each container do one thing well. Based on this principle, one can have separate teams work on different containers as their concerns are segregated well enough.

**Cohesiveness/Reusability** 

With sidecar containers for processing logs, this container can as well be reused at other places in the application.

## Cautions

-- Consider the deployment and packaging format you will use to deploy services, processes, or containers. Containers are particularly well suited to the sidecar pattern.

-- When designing a sidecar service, carefully decide on the interprocess communication mechanism. Try to use language- or framework-agnostic technologies unless performance requirements make that impractical.

-- Before putting functionality into a sidecar, consider whether it would work better as a separate service or a more traditional daemon.

-- Also consider whether the functionality could be implemented as a library or using a traditional extension mechanism. Language-specific libraries may have a deeper level of integration and less network overhead.

## Other

###### Use this pattern when:

-- Your primary application uses a heterogeneous set of languages and frameworks. A component located in a sidecar service can be consumed by applications written in different languages using different frameworks.

-- A component is owned by a remote team or a different organization.

-- A component or feature must be co-located on the same host as the application
You need a service that shares the overall lifecycle of your main application, but can be independently updated.

-- You need fine-grained control over resource limits for a particular resource or component. For example, you may want to restrict the amount of memory a specific component uses. You can deploy the component as a sidecar and manage memory usage independently of the main application.

###### This pattern may not be suitable:

-- When interprocess communication needs to be optimized. Communication between a parent application and sidecar services includes some overhead, notably latency in the calls. This may not be an acceptable trade-off for chatty interfaces.

-- For small applications where the resource cost of deploying a sidecar service for each instance is not worth the advantage of isolation.

-- When the service needs to scale differently than or independently from the main applications. If so, it may be better to deploy the feature as a separate service.

## Reference URL

[Container Designs](https://vitalflux.com/container-design-patterns-kubernetes-pods-design/)

[Side Car Pattern](https://docs.microsoft.com/en-us/azure/architecture/patterns/sidecar)

[Side Car Pattern Example](https://medium.com/@thanhtungvo/build-git-sync-for-side-car-container-in-kubernetes-4ee51bda84f0)