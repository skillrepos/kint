# Introduction to Kubernetes

**Lab 1- Building Docker Images**

> **Purpose: In this lab, we'll see how to build Docker images from
> Dockerfiles.**

1.  Switch into the directory for our docker work.

> **\$ cd roar-docker**

2.  Do an **ls** command and take a look at the files that we have in
    this directory.

> **\$ ls**

3.  Take a moment and look at each of the files that start with
    "Dockerfile". See if you can understand what's happening in them.

> **\$ cat Dockerfile_roar_db_image**
>
> **\$ cat Dockerfile_roar_web_image**

4.  Now let's build our docker database image. Type (or copy/paste) the
    following command: (Note that there is a space followed by a dot at
    the end of the command that must be there.)

> **\$ docker build -f Dockerfile_roar_db_image -t roar-db .**

5.  Next build the image for the web piece. This command is similar
    except it takes a build argument that is the war file in the
    directory that contains our previously built webapp.

> (Note the space and dot at the end again.)

**\$ docker build -f Dockerfile_roar_web_image \--build-arg
warFile=roar.war -t roar-web .**

6.  Now, let's tag our two images for our local registry (running on
    localhost, port 5000). We'll give them a tag of "v1" as opposed to
    the default tag that Docker provides of "latest".

> **\$ docker tag roar-web localhost:5000/roar-web:v1**
>
> **\$ docker tag roar-db localhost:5000/roar-db:v1**

7.  Do a docker images command to see the new images you've created.

> **\$ docker images \| grep roar**

8, Let's go ahead and push our images over to our local registry so
they'll be ready for Kubernetes to use.

> **\$ docker push localhost:5000/roar-web:v1**
>
> **\$ docker push localhost:5000/roar-db:v1**
>
> [END OF LAB]{.underline}

**Lab 2 - Exploring and Deploying into Kubernetes**

**Purpose:** In this lab, we'll start to learn about Kubernetes and its
object types, such as nodes and namespaces. We'll also deploy a version
of our app that has had Kubernetes yaml files created for it.

1.  Before we can deploy our application into Kubernetes, we need to
    have appropriate Kubernetes manifest yaml files for the different
    types of k8s objects we want to create. These can be separate files
    or they can be combined. For our project, there is a combined one
    (deployments and services for both the web and db pieces) already
    setup for you in the caz-class/roar-k8s directory. Change into that
    directory and take a look at the yaml file there for the Kubernetes
    deployments and services.

> **\$ cd \~/caz-class/roar-k8s**
>
> **\$ cat roar-complete.yaml**

See if you can identify the different services and deployments in the
file.

2.  We're going to deploy these into Kubernetes into a namespace. Take a
    look at the current list of namespaces and then let's create a new
    namespace to use.

> **\$ kubectl get ns**
>
> **\$ kubectl create ns roar**

3.  Now, let's deploy our yaml specifications to Kubernetes. We will use
    the apply command and the -f option to specify the file. (Note the
    -n option to specify our new namespace.)

**\$ kubectl -n roar apply -f roar-complete.yaml**

After you run these commands, you should see output like the following:

deployment.extensions/roar-web created

service/roar-web created

deployment.extensions/mysql created

service/mysql created

4.  Now, let's look at the pods currently running in our "roar"
    namespace (and also see their labels).

> **\$ kubectl get pods -n roar \--show-labels**

Notice the STATUS field. What does the "ImagePullBackOff " or
"ErrImagePull" status mean?

5.  Let's check the logs of the pod to learn more about what's going on.
    Run the command below to see the logs of the database pod (note
    again that we add the -n option to specify the namespace):

> **\$ kubectl logs** **-l app=roar-db -n roar**

6.  The output here confirms what is wrong -- notice the part on "trying
    and failing to pull image" or \"image can\'t be pulled\". To get the
    overall view (description) of what's in the pod and what's happening
    with it, we'll use the "describe" command. Use the command below.

> **\$ kubectl -n roar describe pod -l app=roar-db**

7.  Near the bottom of this output, notice the "Events" messages:

Events:

Type Reason Age From Message

\-\-\-- \-\-\-\-\-- \-\-\-- \-\-\-- \-\-\-\-\-\--

Normal Scheduled 7m24s default-scheduler Successfully assigned
roar/mysql-78dc5bc997-d77tf to minikube

Normal Pulling 5m48s (x4 over 7m20s) kubelet, minikube Pulling image
\"localhost:5000/roar-db-v1\"

Warning Failed 5m48s (x4 over 7m20s) kubelet, minikube Failed to pull
image \"localhost:5000/roar-db-v1\": rpc error: code = Unknown desc =
Error response from daemon: manifest for localhost:5000/roar-db-v1 not
found

Warning Failed 5m48s (x4 over 7m20s) kubelet, minikube Error:
ErrImagePull

Warning Failed 5m35s (x7 over 7m18s) kubelet, minikube Error:
ImagePullBackOff

Normal BackOff 2m17s (x21 over 7m18s) kubelet, minikube Back-off pulling
image \"localhost:5000/roar-db-v1\"

8.  Remember that we tagged the images for our local registry **as
    localhost:5000/roar-db:v1** and **localhost:5000/roar-web:v1** .But
    if you scroll back up and look at the "Image" property in the
    describe output, you'll see that it actually specifies
    "localhost:5000/roar-db-v1".

> ![](media/image1.png){width="5.116839457567804in"
> height="2.60410542432196in"}

9.  It is looking for an image with the "-v1" as part of the name. But
    that's not what we tagged ours as. To fix this, edit the
    roar-complete.yaml file and modify the "Image" properties to change
    the "-" to a ":" for the web image (only). Let's see if this fixes
    the problem. Still in the caz-class/roar-k8s directory:

> **\$ gedit roar-complete.yaml**

In the editor, change line 19 from

> **image: localhost:5000/roar-web-v1**

to

> **image: localhost:5000/roar-web:v1**

Also change line 70 from

> **image: localhost:5000/roar-db-v1**

to

> **image: localhost:5000/roar-db:v1**

10. After you make your changes, save the file and close the editor.
    Now, in the other terminal window, start a command to watch the pods
    (the -w option) so we can see when changes occur.

> **\$ kubectl get pods -n roar -w**

11. Now, in the second emulator window (the one in the roar-k8s
    directory), run a command to apply the changed file.

> **\$ kubectl apply -n roar -f roar-complete.yaml**

12. Observe what happens in the window with the watched pods afterwards.
    You should be able to see Kubernetes terminating the old pod and
    starting up a new one. Eventually the new one should show as
    running.

![](media/image2.png){width="5.47394028871391in"
height="2.8562740594925633in"}

> 13\. Even though we did not directly change the deployment, this
> should have fixed that also. You can verify by looking at the
> deploy(ments) again.
>
> **\$ kubectl get deploy -n roar**
>
> 14\. With everything running, we can now actually look at the
> application running (in Kubernetes). Get a list of services for our
> namespace.
>
> \$ **kubectl -n roar get svc**
>
> 15\. Note that the type of service for roar-web is "NodePort". This
> means we have a port open on the Kubernetes node that we can access
> the service through. Find the nodePort under the PORT(S) column and
> after the service port (8089) and before the "TCP". For example, if we
> have **8089:31790/TCP** in that column, then the actual nodePort we
> need is **31790**.
>
> 16\. In the web browser, go to the url below, substituting in the
> nodePort from the step above for "\<nodePort\>". You should see the
> running application.
>
> [**http://localhost:\<nodePort\>/roar/**](http://localhost:%3cnodePort%3e/roar/)

[END OF LAB]{.underline}
