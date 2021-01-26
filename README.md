# Envoy Monitoring By ZABBIX

[![N|Solid](https://www.cfcim.org/wp-content/uploads/members/14191/14191-ZEN%20NETWORKS-2017-12-16-23-37-38-194x150.png)](https://www.zen-networks.ma/)

This Template is created by [Zen Networks](https://www.zen-networks.ma/) for envoy proxy monitoring Using ZABBIX.

The about Envoy version which we have tested the template.
  -  source revision : 4ff61b89ae24a0c86a4c1a478cc7c3886f389747
  - release number : 1.17.0-dev
  - status of the source tree at the build time : clean-getenvoy-a5345f6-envoy
  - build mode : RELEASE
  - TLS library : BoringSSL

ZABBIX version used in the test is 4.4 


# Setup the template:
Just import the template to ZABBIX and change the macro for your environment.
  - we assume that status port of envoy is 5000, that could be changed by editing the Macros {$STATUS.PORT}
  - there's 3 additional Macros: {$ENVOY.STATUS.PATH} which for the page that we consultant to get the parameters,{$STATUS.SCHEME} = http , and finally {$CLUSTERS.STATUS.PATH} used for clusters page.

### About the template
We are using auto-discovery of all clusters exist behind envoy proxy and getting their parameters, and response codes for each listener, and some envoy parameters.
In the template there are two discovery rules: 
- The first one is for cluster name and parameters discovery, with two LLD macro ({#NAME} and {#PRM}), we are using HTTP Agent to get those variables in JSON form. And we've created a prototype item that gets parameters from a JSON list by using the two variables. 
- The second one is for listener discovery, with one LLD macro ({#LISTENER}), that use HTTP Agent to get parameters from the status page of the envoy. We used those listener name in order to get response code for each cluster. Then we've created five item prototype for each response code (1xx, 2xx, 3xx, 4xx, 5xx), the second item prototype (2xx) has a trigger as a ratio < 95 % 

The template contains 11 items as dependent items and 3 other that use HTTP Agent to get parameters from the status page of envoy in JSON form.
You could add new items by just consulting the "envoy: clusters_stats" item that contains all parameters of envoy in JSON form.
