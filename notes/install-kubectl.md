# install kubectl

1.  Update the `apt` package index and install packages needed to use the Kubernetes `apt` repository:

    ```shell
    sudo apt-get update
    sudo apt-get install -y ca-certificates curl
    ```

    If you use Debian 9 (stretch) or earlier you would also need to install `apt-transport-https`:

    ```shell
    sudo apt-get install -y apt-transport-https
    ```
2.  Download the Google Cloud public signing key:

    ```shell
    sudo curl -fsSLo /etc/apt/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
    ```
3.  Add the Kubernetes `apt` repository:

    ```shell
    echo "deb [signed-by=/etc/apt/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
    ```
4.  Update `apt` package index with the new repository and install kubectl:

    ```shell
    sudo apt-get update
    sudo apt-get install -y kubectl
    ```

**Note:** In releases older than Debian 12 and Ubuntu 22.04, `/etc/apt/keyrings` does not exist by default. You can create this directory if you need to, making it world-readable but writeable only by admins.
