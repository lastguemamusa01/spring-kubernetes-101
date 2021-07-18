# spring-kubernetes-101



Container orchestration provide us : 

-	When there are a lot of instances per microservices

-	Auto scaling

-	Service discovery – help microservices find one another

-	Load balancer – distribute load among multiple instances of microservice

-	Self healing – do health checks and replace failing instances

-	Zero downtime deployments – realese new versions without downtime


 
 ![image](https://user-images.githubusercontent.com/25869911/126052675-d7085f6a-03a4-44db-917c-e0f98ba1e9ac.png)

 
 


Aws container orchestration  : 
ECS – elastic container servide
Fargate – serverless version of aw secs

Cloud neutral options(aws, cloud, datacenter, azure, gcp) – Kubernetes (anywhere)
Aws – elastic Kubernetes services(cost)
Azure – azure Kubernetes service(cost)
Gcp – google Kubernetes engine (only free)

Docker image  has all  package so, framework, language, configurations, etc. Locahost, datacenter, cloud. Run wherever you want.

Kubernetes – cluster – group of servers – automatically montiring your applications

Create Kubernetes clusters can be really though – for this use GKE.  

Kuberentes – clusters, nodes, pods, containers, deployment, services, replica set, and a host of other features.

Gcp – regions, zones, projects, compute engines


Deploy simple rest api to Kubernetes





Cluster

![image](https://user-images.githubusercontent.com/25869911/126052682-dad426e9-fe55-4f10-9459-ec2f0081b024.png)

 

Kubernetes – best resource manager – manage virtual servers(cloud)

Equivalents –

Amazon call aws ec2 – elastic compute cloud
Azure – virtual machines
Gcp – compute engine
Kubernetes - nodes

Manage thousand of nodes – there are master nodes(manage cluster) and worker nodes(run your applications)

Cluster – combination of worker nodes or nodes and the master node(ensure that the nodes are available). Master nodes manage the worker nodes.

Enable Kubernetes engine

In the gcp console – in the search – Kubernetes engine -> enable -> create 

Menu of the Kubernetes engine

Clusters – where you can create clusters and manage them
Workloads- where you can manage the appliations or the containers that you want to deploy into the cluster

Services & ingress – some of your workloads actually be rest api or web applications. Youd want to provide access to external world to these workloads. Services give external world access to your applications which are deployed into Kubernetes clusters. 


Configuration – to store your application configuration.

Storage – to provide persistent data storage for you application






Creating cluster – need to choose how powerful you cluster is and where it should be located, type of nodes you want, number of nodes, their location.

Delete cluster when not using it, its expensive.

Easy to create cluster – background do a lot of stuff. Simple is the best

Kubernetes is K8S – Kubernetes 8 letters and s

In clusters menu – you can see cluster and node information
Kubernetes take memory to manage to nodes


1 – connect to Kubernetes cluster – command line, google cloud shell
	Go to inside cluster name – in the right superior you can click command icon to activate cloud shell – cloud sheel opens in the browser or gcp
	Click the superior icon with name connect – you can run command or copy paste to run it.
	Now you are in the cluster console – 

kubectl – kube contoroller – Kubernetes command – this commando is like gcp or ng. this work any Kubernetes cluster – use to deploy application, increase number of instances of your app,  this already instaled in cloud shell


          kubectl version – to see version of Kubernetes – this show client and server version

deply an application kuberentes cluster – 

          kubectl create deployment hello-world-rest-api --image=in28min/hello-world-rest-api:0.0.1.RELEASE

the image is from docker hub from the docker hub repository and tag. 

Expose this deployment to outside of the world - 

          kubectl expose deployment hello-world-rest-api --type=LoadBalancer --port=8080

go to menu services&ingress – to check the service – status need to be ok

http://34.132.207.71:8080/hello-world    - is alive


what happening in the background ?

        kubectl get events

to see the deployment process – when exposed a simple deployment and the Kubernetes magically created for us a pod, a replica set, deployment, service. This is now available recently versions


          kubectl get pods or kubectl get pod.  – can be plural or singular – last element

-	You can see the pods created

          kubectl get replicaset 

-	You can see replicaset

          Kubectl get service – you can see services


Kubernetes uses single responsibility principle – one concept, one responsibility

          Kubectl create deployment -> create deployment, replicaset and pod

          Kubectl expose deployment -> create service

Deployment, Pod , replicaset and service is for : enable manage workloads, to provide external access to workloads, to enable scaling, and to enable zero downtime deployments


Pods – is the smallest deployable unit in Kubernetes. Your container lives inside a pod. Pods can have multiple containers. Cotainers share resoruces in the pod and can talk each other containers using localhsot

          kubectl get pods -o wide.   – you can see pod with ip address

          kubectl explain pods.  – explain info about pod

kubectl get pods and kubectl describe pod name(pod)
 
          kubectl describe pod hello-world-rest-api-687d9c7bc7-prclj.   – to see specific info

pods have :

namespace is important – to provides isolations for parts of the cluster, from other parts of cluster. Namespace create for dev and q&a

labels – Kubernetes, link theses together by selectors and labels – important

annotations = meta information specific pod like release id, build id, author name, etc

status = running , show what status


pods – group containers and categorizations by labels


 
![image](https://user-images.githubusercontent.com/25869911/126052686-3ad55437-4cb3-4f7b-9c44-9612fb5952ad.png)






 
Replica set :

Replicaset- ensure that a pecific number of pods are running at all times. Desired number of pods 1, current pod 1 and ready pod 1. 


        kubectl get pods -o wide. – see more details of pods


kubectl delete pods name

          kubectl delete pods hello-world-rest-api-687d9c7bc7-prclj. – kill or delete the pod

          kubectl get pods -o wide – there is another pod automatically launched

replicaset always monitoring pods and if there less number of pods needed, it created , one is missing and start new one.

How to tell replicaset that how many pod we need ?

          kubectl scale deployment hello-world-rest-api --replicas=3

message :  deployment.apps/hello-world-rest-api scaled

          kubectl get pods. -> now we have 3 pods


          kubectl get events --sort-by=.metadata.creationTimestamp  – see events order by times

replicaset creating more pods to scale up

            kubectl explain replicaset – to see info



deployment in Kubernetes : 

update new version v1 -> v2, for zero downtime

        kubectl get rs -o wide – rs es replicaset, ver mas especifico como docker images


kubectl set image deployment nameDeployment containerName=imageName

        kubectl set image deployment hello-world-rest-api hello-world-rest-api=DUMMY_IMAGE:TEST     
         
- application go down tipically, even if we mistake, the application is still running

MESSAGE : deployment.apps/hello-world-rest-api image updated

        Kubectl get rs -o wide.  – we can see 2 replicaset one is running, another one is not ready because this have errors. Image path is wrong

        kubectl get pods – we can see new one status for pod is InvalidImageName

        kubectl describe pod podName – you can details for specific pod – you can see the error message


when you see the events – also que see the error


![image](https://user-images.githubusercontent.com/25869911/126052688-8acbffc7-4754-4097-b10c-102401cd7d37.png)

 

        kubectl set image deployment hello-world-rest-api hello-world-rest-api=in28min/hello-world-rest-api:0.0.2.RELEASE     
        
- the correct version of version 2 of docker hub  

right now this switches to v1 to v2, in the middle of switching can show v1 and v2, but after all this will be changed to v2. Scale up the v2 and scale down and delete to v1.

Strategy for deployment is called rolling updates. 5 instances in v1 and v2, update one pod at a time, launch one pod v2 and its up and running, it reduces the number of pods for v1. There are another stragies also like 50%/50%.

Pods – wrappers for containers.
Replicaset- ensures tha a specific number of pods are always running. Instances of pods
Deployment – ensures that a releasee upgrade, a swtich from v1 to v2, happens wiout a hitch. Without downtimes.


Service in Kubernetes – 


                kubectl get pods -o wide – each pods has unique ip

http://34.132.207.71:8080/hello-world   - this distributed amoung the pods

kill one of the pod

                kubectl delete – can kill or delete pod, replicaset, deployment


for the consumer you need give then only one url and the background the pods having dynamic urls. This happening because service

service – provide an always available external interface to the applications which are running inside the pods. Basically allows your application to receive traffic through a permanent lifetime ip address.

You can see in the menu in gcp services &ingress -> you can see the endpoint.
You can enter the service to see info

In gcp -> seach load balancing
-	You can see specific load balancer created for this service
-	You can see front end and backend(pods)


Clear – for erase what you have done in console


                  kubectl get services – to see all services

Kubernetes is cluster ip service – can only be accessed from inside the cluster. No external ip. If you have any services which are directly consumed inside you cluster, you can have them use a clusterIp.



In the gcp, Kubernetes engine in workloads – screen
With UI with the gcp you can set things, but you can use console

In workloads in actions you can do – autoscale, expose, rolling update and scale
In edit -> you can edit yaml file

You can see details, revision history, events



Kubernetes architecture 



![image](https://user-images.githubusercontent.com/25869911/126052692-c37894aa-e81d-47a6-bb04-1c350c75f458.png)

 

Master node – ETCD(distributed database) 

All configuration changes, services, deployments that we are creating, all the scaling operations is stored in distributed database.
Console commands -> 4 instances for app a , 10 instances for app b -> desired state is store in distributed database.

-	Have 3 or 5 replicas of distrubted database, for assure Kubernetes cluster state is not lost.

API Server – with console kubectrl -> the changes is done through API Server, change is submitted in the api server and process it.

Scheduler –  responsible scheduling the pods on to the nodes. Kubernetes cluster have several nodes. Scheduler -When creating a new pod, decide which node the pod has to be scheduled. The decision is based on how much memory is available, on how much cpu is available, are there any port conflicts and other factors.

Controller manager – manages the overall health of the cluster. When we are executing kubectl commands updating desired state, kubectl manager make sure that whatever desired state that we have, like 10 instances A app and 10 instances b app, all those changes needs to be executed into the cluster and the controller manager is responsible for that. Make sure that actual state of the Kubernetes cluster matches the desired state.




![image](https://user-images.githubusercontent.com/25869911/126052696-06a28706-c891-40be-adcb-ace96d49fee9.png)

 






Personal app not going to scheduled on to the master node, this is in worker node.



![image](https://user-images.githubusercontent.com/25869911/126052697-ce4c4709-3378-46e1-a7ae-ba22f431b7ea.png)





 
Node or worker node

User app -> running pods inside worker nodes or node
User app is ruuning insides PODS
Sigle node has several pods, pods has containers

Node agent called kublet – make sure that it monitors what is happening on the node and communicates it back to the master node. When pod goes down, node agent report to controller manager(master)

Networking component called kube-proxy – expose a service is possible through networking component. Helps you exposing services around your nodes and you pods.

Container runtime (CRI -docker, rkt etc)  - to run containers inside our pods and these need the continer runtime. Docker used frequently container runtime. You can use Kubernetes with any OCI(open container interface), runtime spec implementations

App is still running even master node is going down. Master node – for configuration. Only you cannot make changes.


Command(cloud shell) : kubectl get componentstatuses

You can see all the components like controller manager, etcd, scheduler of master node.



Installing gcloud – from the terminal on pc. You can use gcloud(command line interface to google cloud) or kubectrl(command line interface to kubernetes)
  
          Command - gcloud auth login 

Installing kubectl – to use kubectl in the terminal on pc.


You can connect the cluster copying the gcloud command and excute in terminal.

kubectl version(to know if you have kubectl client installed) – will show client and server version

in local you can configure yaml files, git version control, better performance.


for the rest api project you need specific name and version of the rest api. You don’t need spring cloud stater config and netlifx eurkea client in the pom.xml file because kubernet have own configuration management using config maps(starter config) and have own service discovery(eureka), and have own. We don’t need also spring cloud sleuth zipkin and spring rabbit, Kubernetes has own logging and tracing in pom xml.


Also in the controller for the logger is changed to simple log for Kubernetes, and you need to add Host info name and version in the currencyExhcangeController.java 

CurrencyConversionControlle.java

        //CHANGE-KUBERNETES
        logger.info("calculateCurrencyConversion called with {} to {} with {}", from, to, quantity);

currencyExhcangeController.java

        //CHANGE-KUBERNETES
        String host = environment.getProperty("HOSTNAME");
        String version = "v11";



For the spring actuator you need to add health check in the Kubernetes, add in the application properties of the currency exchange service.
      
        ## CHANGE-KUBERNETES
        management.endpoint.health.probes.enabled=true
        management.health.livenessState.enabled=true
        management.health.readinessState.enabled=true

currencyExchangeProxy.java add the hardcoded url

        //CHANGE-KUBERNETES
        @FeignClient(name = "currency-exchange", url = "${CURRENCY_EXCHANGE_SERVICE_HOST:http://localhost}:8000")
        //@FeignClient(name = "currency-exchange", url = "${CURRENCY_EXCHANGE_URI:http://localhost}:8000")
        public interface CurrencyExchangeProxy {


first try using currency exchange service host, it there are no host use localhost

        CURRENCY_EXCHANGE_SERVICE_HOST:http://localhost

2 – build the containers y push to the docker registry

In the pom.xml file of both rest api change the name of your docker hub account name

        <build>
            <plugins>
                <plugin>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-maven-plugin</artifactId>
                    <configuration>
                        <image>
                            <name>many2352/mmv2-${project.artifactId}:${project.version}</name>
                        </image>
                        <pullPolicy>IF_NOT_PRESENT</pullPolicy>
                    </configuration>                
                </plugin>
            </plugins>
        </build>



      Run command mvn -N io.takari:maven:wrapper

– in the rest api project if you don’t have mvnw file, create mvnw file

      Run the command ./mvnw spring-boot:build-image -DskipTests. 

Copy the image id and link

Command  to push to the docker hub

      docker push many2352/mmv2-currency-conversion-service:0.0.11-SNAPSHOT

To push your image to your docker hub

Use java 11 not 15, java 15 fucked

-	To deploy the docker hub container, you need to enter access gcp and Kubernetes in the terminal checking

        Command : kubectl version.  – see the local and remote server connected and presented

        Command : kubectl create deployment currency-exchange --image= many2352/mmv2-currency-exchange-service:0.0.11-SNAPSHOT 

After that we need to create the service

        Command : kubectl expose deployment currency-exchange --type=LoadBalancer --port=8000

        Command : kubectl get services    -> to see if the service is on, you need to check the external ip

        Command : kubectl get pods -> to check If the pod is ready

        Command : kubectl get replicasets -> to check if replicaset is ready

        command : kubectl get all -> too see everything status you have created

to check the external url is alive

        command : curl http://35.202.206.61:8000/currency-exchange/from/USD/to/INR


do this also for currency conversion.

        Command : curl http://34.133.94.25:8100/currency-conversion-feign/from/USD/to/INR/quantity/10

IN THE currencyexchangeproxy.java - > 

        CURRENY_EXHCANGE_SERVICE_HOST

Servicename_service_host -> default environment variable kubernete automatically created when you launch up new pods.

currencyExchangeController.java.  -> HOSTNAME -> is default environment variable in Kubernetes. -> is the pods name.


in currency conversion you can see currency exchange pods name

Kubernetes also provides a declarative configrutaion of deploying applications. ->YAML -> state you want

          Command : kubectl get deployments. -> see all deployments

Create yaml file for all of this

In the command go to the currency-exchange folder and

          Command: kubectl get deployment currency-exchange -o yaml.  -> you can see all configuration yaml file

          Command : kubectl get deployment currency-exchange -o yaml >> deployment.yaml -> this will create the yaml file of deploy.

          Command : kubectl get service currency-exchange -o yaml >> service.yaml -> this will create the yaml file of service.


You can copy all thing in the service and add in the deployment.yaml
After

          Deployment code
          ---
          Service code


You can customize the deployment.yaml

          Replicas:2

          Command : pwd. -> what directory I am

          Command: kubectl diff -f deployment.yaml.   -> show me the differences from the server config right now and my deployment configuration yaml file

          Command: kubectl apply -f deployment.yaml b. -> apply the gcp Kubernetes this configuration deployment yaml file.

Declarative approach – configure your server by yaml file

          Command: watch curl http://34.133.94.25:8100/currency-conversion-feign/from/USD/to/INR/quantity/10    -> each 2 seconds request and see the result


Load balancing and service discovery is automatically done


Simply the deployment – delete what we don’t need it


Inside the pods, there are containers – we have image
In the containers we have multiple images

RollingUpdate -> strategy for the updating from v1 to v2 , how you want to do it

In the service define ports, sessionAffinity -> not for rest api, sessionAffinity is for app -> sed all the requests to the same instance. Type of service is loadbalancer


Logging and tracing related to your microservices , to see that information. You need to enable api.
Go to GCP -> api and services -> enables apis and services

Search cloud logging api -> you can enable or disable this feature

Search stackdriver

Enable stackdrive api, stackdriver monitoring api, cloud trace api, stackdriver error reporting api and stackdriver profiler api

-	To delete deployment

          Command : kubectl delete all -l app=currency-exchange    -> borra todo lo que se llame currency exchange -> delete pod , service and deployments. replicasets


          Command : kubectl get all – to check if all erased

          Command : kubectl apply -f deployment.yaml.  -> to deploy with configuration file, before always erase the version before. This deployment is for using the kubectl apply

          Command : kubectl get services  --watch.  -> to see each 2 seconds if the services created external ip



          Command : curl http://35.202.241.100:8000/currency-exchange/from/USD/to/INR

You can change configuration deployment.yaml file 

          Command: kubectl apply -f deployment.yaml


-	Deploy currency conversion

Go to the local file directory of folder deploy currency conversion that deployment.yaml file founded.  On terminal

          Command : kubectl apply -f deployment.yaml

This will create the pod in workload and service

          Command: kubectl get svc   -> to see if the service up proporsioning external ip

Google shell : 

          Command : curl http://34.133.124.37:8100/currency-conversion-feign/from/USD/to/INR/quantity/10

You can see the response

Deploy declarative way, load balancing and service discovery already there

-	Creating environment variables to enable microservice communication

The problem of the approach using CURRENCY_EXCHANGE_SERVICE_HOST(SERVICE_NAME_SERVICE_HOST)

Start currency conversion but currency exchange is not available, in this case currency conversion cannot get the CURRENCY_EXCHANGE_SERVICE_HOST environment variable. Only live service can provide, so use the custom environment variables instead using default environment variables of Kubernetes



In currencyExchangeProxy.java

CUSTOM_EXCHANGE_URI is custom environment variable

        @FeignClient(name = "currency-exchange", url = "${CURRENCY_EXCHANGE_URI:http://localhost}:8000")
        public interface CurrencyExchangeProxy {

when we depoying our service populate with this environment variable

-	When you change the code and you want to deploy docker image to docker hub, change the tag version in the pom.xml file

        <version>0.0.12-SNAPSHOT</version> <!-- CHANGE-KUBERNETES -->
    


create docker image and deploy


in the currency conversion service

        command : ./mvnw spring-boot:build-image -DskipTests

this will create the image of docker

in pml.xml of currency exchange service, update pom.xml version to .12

        version>0.0.12-SNAPSHOT</version> <!-- CHANGE-KUBERNETES -->

update the version also in here
currencyExchangeContrller.java

        //CHANGE-KUBERNETES
        String host = environment.getProperty("HOSTNAME");
        String version = "v12";


To deploy to docker hub

        Command : docker push many2352/mmv2-currency-conversion-service:0.0.12-SNAPSHOT

        Command : docker push many2352/mmv2-currency-exchange-service:0.0.12-SNAPSHOT


In the deployment.yaml from currency-conversion-service, change the version
deployment.yaml

      containers:
        - image: in28min/mmv2-currency-conversion-service:0.0.12-SNAPSHOT


Go to the currency-conversion-service folder

      Command : kubectl diff -f deployment.yaml   - to see the difference from this file and the file in the gcp

      Command : kubectl get svc   - copy name of the service of currency-exchange

In the deployment.yaml add the environment variable

      spec:
            containers:
            - image: in28min/mmv2-currency-conversion-service:0.0.12-SNAPSHOT
              imagePullPolicy: IfNotPresent
              name: mmv2-currency-conversion-service
              env:
                - name: CURRENCY_EXCHANGE_URI
                  value: http://currency-exchange


Deploy:

            Command: kubectl apply -f deployment.yaml

Message:

deployment.apps/currency-conversion configured
service/currency-conversion unchanged

            command: kubectl logs <pods name>. -> kubectl logs currency-conversion-6494dc4cbf-hnnxp

-	You can see the logs of the deployment in the container spring boot boot process

            Command : kubectl logs -f currency-conversion-6494dc4cbf-hnnxp

-	You can follow the logs upcoming to see what happen inside

In the cloud shell

            Command : curl http://34.133.124.37:8100/currency-conversion-feign/from/USD/to/INR/quantity/10

We see reponse but v11, not v12 , because we are not deployed the new version

Or instead of the custom variable in the deployment.yaml file, you can centralized configuration option

-	Centralized configuration in Kubernetes –

You can use configmap – create centralized configuration putting the custom environment as literal

            Command : kubectl create configmap currency-conversion --from-literal=CURRENCY_EXCHANGE_URI=http://currency-exchange

            Command : kubectl get configmap   – to see the all configmap

            Command: kubectl get configmap currency-conversion – to see that configmap

            Command : kubectl get configmap currency-conversion -o  yaml  – to see yaml configuration

You can sava as yaml file

            Command : kubectl get configmap currency-conversion -o  yaml   >> configmap.yaml

We removed something that we don’t need it

  Result : 
  
  
            apiVersion: v1
            data:
              CURRENCY_EXCHANGE_URI: http://currency-exchange
            kind: ConfigMap
            metadata:
              name: currency-conversion
              namespace: default

  
add this to deployment.yaml file


            ---
            apiVersion: v1
            data:
              CURRENCY_EXCHANGE_URI: http://currency-exchange
            kind: ConfigMap
            metadata:
              name: currency-conversion
              namespace: default


remove or comment the hardcoded values of environment in the deployment.yaml

            #        env:
            #          - name: CURRENCY_EXCHANGE_URI
            #            value: http://currency-exchange

Instead of this add the environment from

            envFrom:
                    - configMapRef:
                        name: currency-conversion


deploy again

            Command: kubectl apply -f deployment.yaml


Service discovery, load balancing, config maps(centralized configuration) have in Kubernetes

Centralized logging in Kubernetes

Every point in second fire or request this and you can see

            Command : watch -n 0.1 curl http://34.133.124.37:8100/currency-conversion-feign/from/USD/to/INR/quantity/10


In the gcp -> Kubernetes engine -> cluseters -> view logs or view GKE dashboard in feature body

View logs -> for defauolt is Kubernetes cluster, clear, go to Kubernetes container -> go to pod name and choose the currency-exchange-fw23 pod

Click button jump to now button ->

Copy the textpayload : 

          2021-07-17 23:34:06.432 INFO [currency-conversion,f0cdbddfee04115b,f0cdbddfee04115b] 1 --- [nio-8100-exec-7] c.i.m.c.CurrencyConversionController : calculateCurrencyConversionFeign called with USD to INR with 10

The second is the trace id, trace id is the assigned by cloud sleuth
In pom xml we hace spring-cloud-starter-sleuth

[currency-conversion,f0cdbddfee04115b,f0cdbddfee04115b]

With this trace id : f0cdbddfee04115b

In the gcp log explorer

Paste the query and click run query button  - to search that textPayload

            resource.type="k8s_container"
            textPayload:f0cdbddfee04115b

we can see 2, currency conversion and currency exchange

in the gke dashboard – you can see all the metrics – you can see cpu, disk and memory use, by cluster, namespace, nodes, workloads, Kubernetes services, pods, containers levels. Alerts like falling containers

centralized logging, configuration, service discovery, naming servers, load balancing in Kubernetes have.

Deployment in Kubernetes

Applications have new deployment or releases every week or month

Ensure deployment , no downtime, Kubernetes have features for that.

              Command : kubectl rollout history deployment currency-conversion



              There are 3 revisions
              REVISION  CHANGE-CAUSE
              1         <none>
              2         <none>
              3         <none>


Currency exhcnage service has 1 revision

Go to the currency-exchange-service folder and change the deployment.yaml

Lets make some mistake

              spec:
                    containers:
                    - image: Dummy

With wrong image or some error


              Command: kubectl diff -f deployment.yaml

Check the difference

deploy again

              Command: kubectl apply -f deployment.yaml

Check in cloud shell, curl the currency conversion service

currency exchange is still working

              Command : kubectl get pods 

              currency-exchange-6cdb978b8c-vpqcc      0/1     InvalidImageName   0          113s
              currency-exchange-765b4cdf46-8fk67      1/1     Running            0          5d23h

new pods have error, but old version is working

safe propose

              command: kubectl rollout history deployment currency-exchange

you can see 2 revision

for not affecting you can change in the deployment yaml file and excute deployment.yaml file.

Other option is todo undo deployment. , got back to revision 1 because revision 2 was failed
Command: kubectl rollout undo deployment currency-exchange --to-revision=1

Command L kubectl get pods   - you can see the version of deployment is rollback to revision 1, so the new pods failed is disappeared.

Now fix the pom.xml, upgrading the version

              containers:
                    - image: in28min/mmv2-currency-exchange-service:0.0.12-SNAPSHOT

deploy again

              Command: kubectl apply -f deployment.yaml

When switching from one deployment to new deployment – deployment or release to new version – there is a little bit of down time. 15 seconds of down times

Configure liveness and readiness probes –

Kubernetes uses probes to check the health of a microservice 
-	If readiness probe is not successful, no traffic is sent
-	If liveness probe is not successful, pod is restarted.

Spring boot actuator(2.3 high version) provides inbuilt readiness and liveness probes
path
/health/readiness
/health/liveness

Pom.xml we already have actuator

In application.propertie of currency exchange

Add this

              ## CHANGE-KUBERNETES
              management.endpoint.health.probes.enabled=true
              management.health.livenessState.enabled=true
              management.health.readinessState.enabled=true


              command: kubectl get svc   – to see external ip


35.202.241.100:8000/actuator   in browser

{"_links":{"self":{"href":"http://35.202.241.100:8000/actuator","templated":false},"health":{"href":"http://35.202.241.100:8000/actuator/health","templated":false},"health-path":{"href":"http://35.202.241.100:8000/actuator/health/{*path}","templated":true},"info":{"href":"http://35.202.241.100:8000/actuator/info","templated":false}}}


http://35.202.241.100:8000/actuator/health

{"status":"UP","groups":["liveness","readiness"]}


http://35.202.241.100:8000/actuator/health/readiness

{"status":"UP"}


In the deployment.yaml file of currency exchange service
Add : 

      containers:
      - image: in28min/mmv2-currency-exchange-service:0.0.12-SNAPSHOT
        imagePullPolicy: IfNotPresent
        name: mmv2-currency-exchange-service
        readinessProbe:
          httpGet: 
            port: 8000
            path: /actuator/health/readiness
        livenessProbe:
          httpGet: 
            port: 8000
            path: /actuator/health/liveness



      Command: kubectl diff -f deployment.yaml

Check the difference


deploy again

      Command: kubectl apply -f deployment.yaml

Only when container ready will deploy and pass, the old pod will die after the new pod is ok.

-	Autoscalling microservices in Kubernetes

Implement autoscaling – based on load

In the deployment.yaml file in the currency exchange

If the load is high increase the number of pods of currency exchange

 Manually 

        -	Deployment.yaml file replicas:3 or 5

        Command : kubectl scale deployment currency-exchange –replicas=4



-	Automatically –

Autoscale,  you edit minimum and maximum of pods,
If the pod cpu percentage is high than 70 percentage, a new pod create until max of 3 pods


        Command : kubectl autoscale deployment currency-exchange --min=1 --max=3 --cpu-percent=70

This will create horizontalpodautoscaler

        Command : kubectl get hpa   -  to see all horizontal pod autoscaler

        Command : kubectl top pod – to see how many cpu utilization

        Command : kubectl top nodes   - to see cpu and memory of nodes are using

To delete horizontal autoscaler

        Command: kubectl delete hpa currency-exchange

