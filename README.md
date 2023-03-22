# Introduction to Kubernetes quick labs

These instructions will guide you through configuring a GitHub Codespaces environment that you can use to run a basic lab to create containers and one to do a simple deployment in Kubernetes.

These steps **must** be completed prior to starting the actual labs.

## Create your own repository for these labs

- Ensure that you have created a repository by forking the [skillrepos/kint](https://github.com/skillrepos/kint) project as a template into your own GitHub area. You do this by clicking the `Fork` button in the upper right portion of the main project page and following the steps to create a copy in [your-github-userid/kint](https://<your-github-userid>/kint).

## Configure your codespace

1. In your forked repository, start a new codespace.

    - Click the `Code` button on your repository's landing page.
    - Click the `Codespaces` tab.
    - Click `Create codespaces on main` to create the codespace.
    - After the codespace has initialized there will be a terminal present.

## Start your single-node Kubernetes cluster
2. There is a simple one-node Kubernetes instance called **minikube** available in your codespace. Start it the following way:

    - Run the following command in the codespace's terminal (**This will take several minutes to run...**):

      ```bash
      minikube start
      ```

    - The output should look similar to the following.

      ```console
     😄  minikube v1.29.0 on Ubuntu 20.04 (docker/amd64)
✨  Automatically selected the docker driver. Other choices: none, ssh
📌  Using Docker driver with root privileges
👍  Starting control plane node minikube in cluster minikube
🚜  Pulling base image ...
💾  Downloading Kubernetes v1.26.1 preload ...
    > preloaded-images-k8s-v18-v1...:  397.05 MiB / 397.05 MiB  100.00% 115.85 
    > gcr.io/k8s-minikube/kicbase...:  407.18 MiB / 407.19 MiB  100.00% 44.23 M
🔥  Creating docker container (CPUs=2, Memory=2200MB) ...
🐳  Preparing Kubernetes v1.26.1 on Docker 20.10.23 ...
    ▪ Generating certificates and keys ...
    ▪ Booting up control plane ...
    ▪ Configuring RBAC rules ...
🔗  Configuring bridge CNI (Container Networking Interface) ...
    ▪ Using image gcr.io/k8s-minikube/storage-provisioner:v5
🔎  Verifying Kubernetes components...
🌟  Enabled addons: storage-provisioner, default-storageclass
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default
      ```

## Enable a local insecure registry to store images in

3. Enable an addon for minikube to provide a local registry to temporarily store images

    - Run the following command in the codespace's terminal:

      ```bash
      minikube addons enable registry
      ```

    - The output should look similar to the following:

  ```console
    💡  registry is an addon maintained by Google. For any concerns contact minikube on GitHub.
You can view the list of minikube maintainers at: https://github.com/kubernetes/minikube/blob/master/OWNERS
    ▪ Using image gcr.io/google_containers/kube-registry-proxy:0.4
    ▪ Using image docker.io/registry:2.8.1
🔎  Verifying registry addon...
🌟  The 'registry' addon is enabled
  ```

## Labs

Open the labs document by clicking on the link below. (Alternatively, you can go to the file tree on the left, find the file named **labs.md**, right-click on it, and open it with the `Preview` option.) This will open it up in a tab above your terminal. Then you can follow along with the steps in the labs. Any command in the lab that starts with a `$` is intended to be run in the console (without typing the `$`).

Labs doc: [Quick Labs for Introduction to Kubernetes](labs.md)

