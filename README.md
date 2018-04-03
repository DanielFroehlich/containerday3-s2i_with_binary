# Source 2 Image with App Binary (War File Deployment)

Follow these steps to deploy a simple web app using EAP Container onto your OpenShift cluster:

1. Create Project:
```
oc new-project containerday-3 \\
--description "Sample Deployment of an existing JEE App with Java / EAP Source 2 Image build"
```
2. Deploy App:
```
oc new-app \\ https://github.com/DanielFroehlich/containerday3-s2i_with_binary.git \\
--name web-stress-simulator
```
This fails with "no language matched". OpenShift can't determine the lanugage used in the git repo, because there are not hints (like pom.xml or packages.json).

3. So we need give openshift a hint which language to use for Source 2 Image Build. In our case, we use the EAP70 Image:
```
oc new-app \\
   jboss-eap70-openshift:1.6~https://github.com/DanielFroehlich/containerday3-s2i_with_binary.git \\
   --name web-stress-simulator
```
4. Create Route:
```
oc expose svc/web-stress-simulator
```
5. Stress Test using apache bench:
```
ab -n 1000 -c 4 \\
  http://REPLACE_ME/web-stress-simulator-1.0.0/cpu?time=1000
```
Replace URL with the actual one created in the previous step.
If apache bench is not at hand, you could use curl our your browser, too!
