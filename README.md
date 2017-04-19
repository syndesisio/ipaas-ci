# IPaaS CI

Provides an Openshift Template for Jenkins and nexus.

## Installation

Currently there are two flavors provided:

- Ephemeral
- Persistent

### Ephemeral installation

For Jenkins:

     oc create -f jenkins-ephemeral.yml
     oc process jenkins \
     GITHUB_USERNAME=<github username> \
     GITHUB_PASSWORD=<github password> \
     GITHUB_ACCESS_TOKEN=<github access token password> \
     ROUTE_HOSTNAME=jenkins-$(oc project -q).b6ff.rh-idev.openshiftapps.com \
     KUBERNETES_NAMESPACE=$(oc project -q) | oc create -f -


For nexus:

     oc create -f nexus-ephemeral.yml
     oc process nexus \
     ROUTE_HOSTNAME=jenkins-$(oc project -q).b6ff.rh-idev.openshiftapps.com | oc create -f -

### Persistent installation

For Jenkins:

     oc create -f jenkins-pvc.yml
     oc create -f jenkins-persistent.yml
     oc process jenkins \
     GITHUB_USERNAME=<github username> \
     GITHUB_PASSWORD=<github password> \
     GITHUB_ACCESS_TOKEN=<github access token password> \
     ROUTE_HOSTNAME=jenkins-$(oc project -q).b6ff.rh-idev.openshiftapps.com \
     KUBERNETES_NAMESPACE=$(oc project -q) | oc create -f -

For nexus:

     oc create -f nexus-pvc.yml
     oc create -f nexus-persistent.yml
     oc process nexus \
     ROUTE_HOSTNAME=jenkins-$(oc project -q).b6ff.rh-idev.openshiftapps.com | oc create -f -
     
     
For maven builds to use nexus as a mirror, you need to use the appropriate `settings.xml` file.
A secret providing such a file can be created using:

   
    oc create -f m2-settings.yml | oc create -f -


The project also provides a pvc that can be used as a local maven repository:

    oc create secret generic m2-settings --from-file settings.xml

