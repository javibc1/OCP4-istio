# OCP4-Istio
First install Service Mesh (operators configured for OCP 4.5) `oc apply -Rf ./install-operators` and check that operators are installed, pods in running state in projects openshift-operators and openshift-operators-redhat `oc get pods -n openshift-operators && oc get pods -n openshift-operators-redhat`

Next step is to install a istio-cluster `oc apply -f ./install-servicemesh` and wait a bit for all pods to deploy `oc get smcp -n istio-system`. Once pods are running you can deploy bookinfo or recommendation apps by the following commands:

To install the applications:
- Bookinfo: `oc apply -f ./app-bookinfo -n bookinfo`
- Recommendation: `oc apply -Rf ./app-recommendation -n recommendation`
  