# Day 4: Looking at Services

# Services

---

**Why do we need services?**

Each pod gets its own IP address. However, pods in Kubernetes are **ephemeral**; meaning, they come and go very quickly.

Every time a new pod is started, it gets its new IP address.

With a service, you get consistent, stable IP address.

**What is a Service?**

With every object and agent decoupled we need a flexible and scalable
 agent which connects resources together and will reconnect, should 
something die and a replacement is spawned.

Each service is a microservice that handles a particular bit of traffic. Cluster-internal traffic is handled through cluster IP, we can also have NodeIPs and Loadbalancer.

Services are a good alternative for loose coupling components within the cluster and with outside resources.

### Type of Services

By default Cluster IP is used for the service. 

Each pod gets a range of IP addresses.

In the case of a replica, the second pod, would have the same internal IP addresses but a different external.

A Service is an abstraction layer of an IP address, also accessible on a specific port.

**How does a service identify which port to forward the request to?**

A Service identifies pods through selector attributes. In the YAML file for defining the selector; it has a key value part that has to match the key-value pair provided in the deployment. More on this later on.

These labels can be arbitrary names. Key value pairs that identify a set of pods

All the pods that match that key value pair provided by the service are part of the service.

A Service also handles access policies for inbound requests, useful for resource control, as well as for security.

A service, as well as **kubectl**, uses a *selector* in order to know which objects to connect. There are two selectors currently supported:

- **equality-based**Filters by label keys and their values. Three operators can be used, such as **=**, **==**, and **!=**. If multiple values or keys are used, all must be included for a match.
- **set-based**Filters according to a set of values. The operators are **in**, **notin**, and **exists**. For example, the use of **status notin (dev, test, maint)** would select resources with the key of **status** which did not have a value of **dev**, **test**, nor **maint**.

**If a pod has multiple ports open, how does the service know which port to forward the requests to?**

This is defined in the target port attribute provided by the YAML configuration.

If the service is connected to multiple identical pods, the Service will identify which pod is best equipped to handle the request.

If we have a microservice within the cluster such as a MongoDB database, our cluster will be able to talk to the database through that respective IP address.

# Pod-to-Pod Communication

---

While a CNI plugin can be used to configure the network of a pod and 
provide a single IP per pod, CNI does not help you with pod-to-pod 
communication across nodes.

The early requirement from Kubernetes was the following:

- All pods can communicate with each other across nodes.
- All nodes can communicate with all pods.
- No Network Address Translation (NAT).

Basically, all IPs involved (nodes and pods) are routable without 
NAT. This can be achieved at the physical network infrastructure if you 
have access to it (e.g. GKE). Or, this can be achieved with a software 
defined overlay with solutions like:

- [Weave](https://www.weave.works/oss/net/)
- [Flannel](https://coreos.com/flannel/docs/latest/)
- [Calico](https://www.projectcalico.org/)
- [Romana](https://romana.io/).

Most network plugins now support the use of Network Policies, which 
act as an internal firewall, limiting ingress and egress traffic.

For more information see the ["Cluster Networking"](https://kubernetes.io/docs/concepts/cluster-administration/networking/) Documentation page or the list of [networking add-ons](https://kubernetes.io/docs/concepts/cluster-administration/addons/).
