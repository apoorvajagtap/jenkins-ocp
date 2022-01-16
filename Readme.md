Pre-requisites:
- OpenShift cluster deployed.
- Jenkins instance is configured on OpenShift cluster.

Steps:

1. Create a buildConfig & required secret using the YAML definitions available in ./openshift dir:
    
    $ oc create -f openshift/mysecret.yaml
    $ oc create -f openshift/buildConfig.yaml

2. Start the build on OCP:

    $ oc start-build jenkins-cicd-bc

3. Once the build starts, the jobs must show up the status on Jenkins Console.

4. Now, try modify the index.html file, so that it refers to a different GIF (modified to minion-second.gif here).

5. As soon as the index.html is modified and new changes are pushed & commited to the source code repo, the Pipeline on Jenkins should trigger automatically (no need for `oc start-build` after each change).


Note: This repository is an example code to configure Pipeline on Jenkins hosted on OCP.
    : The contents of ./src are inherrited from [monodot/kylie-fan-club](https://github.com/monodot/kylie-fan-club)
