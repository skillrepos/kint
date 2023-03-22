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

    - Run the following command in the codespace's terminal:

      ```bash
      minikube start
      ```

    - Verify the output is similar to below.

      ```console
      $ gh actions-importer version
      gh version 2.14.3 (2022-07-26)
      gh actions-importer        github/gh-actions-importer v0.1.12
      actions-importer/cli       unknown
      ```

## Enable a local insecure registry to store images in

3. Enable an addon for minikube to provide a local registry to temporarily store images

    - Run the following command in the codespace's terminal:

      ```bash
      minikube addons enable registry
      ```

## Labs

In the file tree on the left, find the file named **labs.md**. Right-click on it and open it with the `Preview` option. This will open it up in a tab above your terminal. Then you can follow along with the steps in the labs. Any command in the lab that starts with a `$` is intended to be run in the console.

Labs doc: [Quick Labs for Introduction to Kubernetes](labs.md)

