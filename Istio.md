# SpringBoot Application Virtual Service
```
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: spring-mysql-vs
spec:
  hosts:
  - "*"
  gateways:
  - bookinfo-gateway
  http:
  - match:
    - uri:
       prefix: /
    route:
    - destination:
        host: demo-app-spring
        port:
          number: 8080
 ```

# Kiali Virtual Service (Example to verify ingress gateway but not use in Prod env)

```
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: kiali-vs
spec:
  hosts:
  - "*"
  gateways:
  - bookinfo-gateway
  http:
  - match:
    - uri:
       prefix: /kiali
    route:
    - destination:
        host: kiali.istio-system.svc.cluster.local
        port:
          number: 20001
```
- By default, we will be in a **`default`** namespace. To switch to a particular namespace permenently run the below command.
```
  $ kubectl config set-context $(kubectl config current-context) --namespace=istio-test

  $ kubectl create ns istio-test

  $ kubectl config get-contexts (**` to know current namespace and cluster)
```

# Istio


When infrastructures shifted from monolithic to microservices, containers could bring in consistency and granularity in managing resources. Microservices allowed us to treat one computer as many computers. This enabled applications to scale up and scale down computers depending on demand and reduced wastage of resources. To manage these microservices, the concept of service mesh was implemented. Service mesh is a theory or a model for improving the effectiveness and security of Kubernetes. Istio is an implementation of this service mesh paradigm..

 Istio is an open source service mesh that layers transparently onto existing distributed applications. Istio’s powerful features provide a uniform and more efficient way to secure, connect, and monitor services. Istio is the path to load balancing, service-to-service authentication, and monitoring–with few or no service code changes. Its powerful control plane brings vital features, including features like Dynamic service discovery, Load balancing, TLS termination, HTTP/2 and proxies, Circuit breakers, Health checks, Staged rollouts with%-based traffic split, Fault injection and Rich metrics.

![1](https://github.com/Sri-Learnings/Istio/assets/130881628/184e37c4-1b0c-4d53-85f5-ba26c184aa20)




Istio architecture, the sidecar proxy is called the envoy proxy, which is an independent open source project. Control Plane centrally manages these proxy services using a single daemon istiod. With Istio, operators can inject configurations into the pods as on when necessary.


# What is Istiod


Istiod consolidates the Istio control plane components into a single binary.
Istiod acts as the control plane, distributing the configuration to all sidecar proxies and gateways. It enables intelligent application-aware load balancing from the application layer to other mesh enabled services in the cluster and bypasses the rudimentary kube-proxy load balancing. 

Configuring all the features of a service mesh into a microservices architecture does not involve using each component separately on v1.5 and above. There is no need to adjust deployment and service Kubernetes yaml files to configure Istio. Istiod will centrally manage everything.

# The benefits of putting everything into a daemon i.e. Istiod


1. **`Easy Installation:`** Fewer configurations enable developers to kick-start istio control plane with all features, even for a single pod.

2. **`Easy Configuration:`** in the 1.5 version, we needed multiple configurations to orchestrate a control plane which is now redundant. 

3. **`Easy Maintenance:`** Installing, upgrading, and removing Istio no longer requires a complicated process of version dependencies and startup orders. For example: To upgrade, you only need to start a new istiod version alongside your existing control plane, canary it, and then move all traffic over to it.
Scalability 

4. **`Faster Troubleshooting:`** Fewer components allow for fast environmental debugging.

5. **`Quick Startup time:`** Components no longer need to wait for each other to start in a defined order.
Reduced Resource usage and improved responsiveness: Communication between components becomes guaranteed. Caches can be shared safely, which decreases the resource footprint.
Istiod unifies functionality that Pilot, Galley, Citadel and the sidecar injector previously performed, into a single binary. The unified architecture applies for Istio v 1.5 and above. Before the version 1.5 was released, the architecture comprised multiple components like Pilot, Galley, Citadel and Mixer. 

These components are not needed if we are using a version 1.5 or above. These components in short for our knowledge.

6. **`Mixer :`** Mixer is a platform independent component that enforces access control and policies across the service mesh and collects metrics and logs to be used by other analysis tools like Splunk and DataDog. Read more on how integrating Autopilot with Splunk and Datadog can unlock multiple benefits for your pipeline.

7. **`Pilot :`** Pilot provides service discovery for the Envoy sidecars, traffic management capabilities for intelligent routing (e.g., A/B tests, canary rollouts, etc.), and resiliency (timeouts, retries, circuit breakers, etc.).

8. **`Citadel :`** Citadel enables strong service-to-service and end-user authentication with built-in identity and credential management. 

9. **`Galley :`** Galley is Istio’s configuration validation, ingestion, processing and distribution component. It is responsible for insulating the rest of the Istio components from the details of obtaining user configuration from the underlying platform (e.g. Kubernetes).


# Benefits of using Istio with Kubernetes

** **`Secure cloud-native apps :`** Focus on security at the application level with strong identity-based authentication, authorization, and encryption.

** **`Manage traffic effectively:`** Get fine-grained control of traffic behavior with rich routing rules, retries, failovers, and fault injection. In post production testing chaos monkey integration allows SRE’s to inject delays, faults to improve the robustness . 

** **`Monitor service mesh :`** Itsio provides a service level visibility that allows for tracing and monitoring. This improves troubleshooting. A bottleneck issue without granular level details will take a lot of time to fish out. With service mesh, you can easily break the circuit to failed services to disable non-functioning replicas and keep the API responsive.

** **`Easily deploy with Kubernetes and virtual machines :`** Istio provides visibility and network controls for both traditional and modern workloads including containers and virtual machines. 

** **`Simplify load balancing with advanced features :`** Use automated load balancing for all of your traffic, along with advanced features like client-based routing and canary rollouts. 

** **`Enforce policies :`** Enforce policies with a pluggable policy layer and configuration API that supports access controls, rate limits, and quotas.


![2](https://github.com/Sri-Learnings/Istio/assets/130881628/e3ef84de-94a0-4dea-9fbc-0b1d4f2dba13)



# Istio Setup
https://istio.io/latest/docs/setup/getting-started/

# Download Istio

1. Go to the Istio release page to download the installation file for your OS, or download and extract the latest release automatically (Linux or macOS):
    ```
    curl -L https://istio.io/downloadIstio | sh -
    ```
    *****  The command above downloads the latest release (numerically) of Istio. You can pass variables on the command line to download a specific version or to 
    override the processor architecture. For example, to download Istio 1.18.0 for the x86_64 architecture, run:
    ```
    curl -L https://istio.io/downloadIstio | ISTIO_VERSION=1.18.0 TARGET_ARCH=x86_64 sh -
    ```

2. Move to the Istio package directory. For example, if the package is istio-1.18.0:

  cd istio-1.18.0



3. Add the istioctl client to your path (Linux or macOS):

  ```
  export PATH=$PWD/bin:$PATH
  ```

#  Install Istio

1. For this installation, we use the demo configuration profile. It’s selected to have a good set of defaults for testing, but there are other profiles for production or performance testing.

 ***** If your platform has a vendor-specific configuration profile, e.g., Openshift, use it in the following command, instead of the demo profile. Refer to your platform instructions for details. ****
```
istioctl install --set profile=demo -y
```
2. Add a namespace label to instruct Istio to automatically inject Envoy sidecar proxies when you deploy your application later:
```
kubectl label namespace default istio-injection=enabled
```
# Deploy the sample application

1. Deploy the Bookinfo sample application:
```
kubectl apply -f samples/bookinfo/platform/kube/bookinfo.yaml
```
2. The application will start. As each pod becomes ready, the Istio sidecar will be deployed along with it.
```
kubectl get services
```
 ```
kubectl get pods
```
  Above application running inside the cluster 

# Open the application to outside traffic

 ****  The Bookinfo application is deployed but not accessible from the outside. To make it accessible, you need to create an Istio Ingress Gateway, which maps a 
   path to a route at the edge of your mesh. ****

1. Associate this application with the Istio gateway:
```
kubectl apply -f samples/bookinfo/networking/bookinfo-gateway.yaml
```
2. Ensure that there are no issues with the configuration:
```
istioctl analyze
```
# Determining the ingress IP and ports  

# Verify external access

# View the dashboard
```
Istio integrates with several different telemetry applications. These can help you gain an understanding of the structure of your service mesh, display 
         the topology of the mesh, and analyze the health of your mesh.

Use the following instructions to deploy the Kiali dashboard, along with Prometheus, Grafana, and Jaeger.
```      
1. Install Kiali and the other addons and wait for them to be deployed.
```
kubectl apply -f samples/addons
 ```  
kubectl rollout status deployment/kiali -n istio-system

2. Access the Kiali dashboard.
```
istioctl dashboard kiali
```


# too see trace data

   ***  To see trace data, you must send requests to your service. The number of requests depends on Istio’s sampling rate and can be configured using the 
      Telemetry API. With the default sampling rate of 1%, you need to send at least 100 requests before the first trace is visible. To send a 100 requests to the 
    productpage service, use the following command:
```
for i in $(seq 1 100); do curl -s -o /dev/null "http://$GATEWAY_URL/productpage"; done
```


![3](https://github.com/Sri-Learnings/Istio/assets/130881628/70b0a5f7-8c37-4bf3-a4bb-97c752376db1)


# Uninstall

istioctl x uninstall --purge


```
kubectl delete -f samples/addons

istioctl uninstall -y --purge

kubectl delete namespace istio-system

kubectl label namespace default istio-injection-
```


