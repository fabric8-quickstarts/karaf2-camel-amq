# Camel AMQ QuickStart

This example shows how to use Camel in a Karaf Container using Blueprint to connect to the A-MQ xPaaS message broker on OpenShift.
The Red Hat JBoss A-MQ xPaaS product should already be installed and running on your OpenShift installation - see the [documentation](https://docs.openshift.com/enterprise/3.1/using_images/xpaas_images/a_mq.html)

This example will connect to the A_MQ message broker and send messages to a queue TEST.FOO

### Building

The example can be built with

    mvn clean install


### Running the example in OpenShift

It is assumed that OpenShift platform is already running. If not you can find details how to [Install OpenShift at your site](https://docs.openshift.com/enterprise/3.1/install_config/install/index.html).

When you Deployed the JBoss A-MQ xPaaS product, it should have been assigned a few serivce names based on the broker name and be configured with a user name and password.  The service this application needs to connect to is ${broker-name}-amq-tcp.
To see all the running service names run `oc get services`.  Once you have identified the A-MQ service name, user name, and password you can package and deploy your app on OpenShift using a command similar to the following:

```
mvn fabric8:deploy -Dactivemq.service.name=broker-amq-tcp -Dactivemq.broker.username=admin -Dactivemq.broker.password=admin
```

When the example runs in OpenShift, you can use the OpenShift client tool to inspect the status

To list all the running pods:

    oc get pods

Then find the name of the pod that runs this quickstart, and output the logs from the running pods with:

    oc logs <name of pod>

You can also use the openshift [web console](https://docs.openshift.com/enterprise/3.1/getting_started/developers/developers_console.html#tutorial-video) to manage the
running pods, and view logs and much more.

### Running via an S2I Application Template

Applicaiton templates allow you deploy applications to OpenShift by filling out a form in the OpenShift console that allows you to adjust deployment parameters.  This template uses an S2I source build so that it handle building and deploying the application for you.

First, import the Fuse image streams:

    oc create -f https://raw.githubusercontent.com/jboss-fuse/application-templates/fis-2.0.x.redhat/fis-image-streams.json

Then create the quickstart template:

    oc create -f https://raw.githubusercontent.com/jboss-fuse/application-templates/fis-2.0.x.redhat/quickstarts/karaf2-camel-amq-template.json

Now when you use "Add to Project" button in the OpenShift console, you should see a template for this quickstart. 

