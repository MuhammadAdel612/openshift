# Templates

** In OpenShift, a template describes a set of objects that can be parameterized and processed. A template can be processed to create anything we have permission to create within a given project, for example: pod, services, routes and deployment configurations. A template may also define a set of labels to apply to every object defined in the template.

To getting started, here is a template.yaml template file for our Hello World application looks like...

We can see many of the items already we know: a service, a route and a pod or replicaset with related pod definition. We also see the use of parametric values. These parameters are useful when create a new application.

In OpenShift, templates live in the openshift project. List existing templates **

### [root@master ~]# oc login -u system:admin
### [root@master ~]# oc project openshift

### [root@master ~]# oc get template 
 ** NAME DESCRIPTION PARAMETERS OBJECTS amq62-basic Application template for JBoss A-MQ... 10 (3 blank) 5 ... **

### Add the template we defined before

### [root@master ~]# oc create -f template.yaml 
 ** template "hello-template" created **

### [root@master ~]# oc get template hello-template NAME DESCRIPTION PARAMETERS OBJECTS hello-world-template This is an example of app... 3 (all set) 3

List the parameters that can be override

### [root@master ~]# oc process --parameters hello-world-template 

NAME DESCRIPTION GENERATOR VALUE APPLICATION_DOMAIN The exposed hostname that .. hello-world.cloud.openshift.com INTERNAL_PORT The internal port used by the pods 8080 SERVICE_PORT The port exposed by the service 9000

# Modify an existing template

### [root@master ~]# oc edit template hello-world-template

# Create a Template from Existing Objects

Rather than writing an entire template from scratch, we can also export existing objects in template form, and then modify the template from there by adding parameters and other customizations.

Export existing objects in the project in a template form:

### [demo@master ~]$ oc create -f pod-hello-world-limited.yaml pod "hello-pod" created

### [demo@master ~]$ oc get all NAME READY STATUS RESTARTS AGE po/hello-pod 1/1 Running 0 18s

### [demo@master ~]$ oc export all --as-template=new-template -o yaml > new-template.yaml

# Create an application from a template

OpenShift users can create an application from a previously stored template or from a template file, by specifying the name of the template as an argument.

Create the Hello World application by the above template

### [demo@master ~]$ oc new-app --template=hello-world-template --> Deploying template "openshift/hello-world-template" to project demo

 hello-world-template
 ---------
 This is an example of application template in OpenShift 3

 * With parameters:
    * APPLICATION_DOMAIN=hello-world.cloud.openshift.com
    * INTERNAL_PORT=8080
    * SERVICE_PORT=9000

--> Creating resources ... service "hello-world-service" created route "hello-world-route" created replicationcontroller "hello-world-rc" created --> Success Run 'oc status' to view your app.

When creating an application based on a template, users can set parameter values defined by the template

### [demo@master ~]$ oc new-app --template=hello-world-template -p
APPLICATION_DOMAIN=myapp.cloud.openshift.com
INTERNAL_PORT=8088
SERVICE_PORT=5680
