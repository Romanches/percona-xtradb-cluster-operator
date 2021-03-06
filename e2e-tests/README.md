### prerequirements
CentOS
```
sudo yum -y install epel-release
sudo yum -y install coreutils sed jq curl
curl -s -L https://github.com/openshift/origin/releases/download/v3.11.0/openshift-origin-client-tools-v3.11.0-0cbc58b-linux-64bit.tar.gz \
    | tar -C /usr/bin --strip-components 1 --wildcards -zxvpf - '*/oc' '*/kubectl'
curl -s https://storage.googleapis.com/kubernetes-helm/helm-v2.12.1-linux-amd64.tar.gz \
    | tar -C /usr/bin --strip-components 1 -zxvpf - '*/helm'
helm init --client-only
curl https://sdk.cloud.google.com | bash
```
MacOS
```
brew install coreutils gnu-sed jq kubernetes-cli openshift-cli kubernetes-helm
helm init --client-only
curl https://sdk.cloud.google.com | bash
```
### Build images
```
./e2e-tests/build
```
### Build images and run cluster
```
./e2e-tests/build-and-run
```
### Run tests
```
./e2e-tests/run
```
