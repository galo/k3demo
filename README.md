# k3demo


# Setup Raspian 

Change pi user passwd. 
Enable Wifi 
Enable SSH
Change Host Name

```
raspi-coinfig
```

Add user and add as a sudoer




Install key on hosts

```
ssh-copy-id galo@derr.local
```

# Access RPI Devices

```shell
ssh luxor.local
ssh derr.local
```

# Install tmux

```shell
sudo apt-get install tmux
```

# Setup K3

https://blog.alexellis.io/test-drive-k3s-on-raspberry-pi/

https://github.com/rancher/k3s

# Install K3 binary

```shell
wget https://github.com/rancher/k3s/releases/download/v0.5.0/k3s-armhf && \
  chmod +x k3s-armhf && \
  sudo mv k3s-armhf /usr/local/bin/k3s
```

# Run the cluster

```
sudo k3s server
```

Or to get resource metrics

```
sudo k3s server --kubelet-arg="address=0.0.0.0"
```

You will get an error and had to enable cgorup memory in the device

# Optional - Install Metric Server

To get metrics on K8s, install the receip. I had to update the image since it was arm64 specific, and replace it for an armv7

```
kubectl apply -f k3s/recipes/metrics-server/
```

The you can make call slike

``
kubectl top node
```


# Accessing cluster from outside

Copy /etc/rancher/k3s/k3s.yaml on your machine located outside the cluster as ~/.kube/config. Then replace "localhost" with the IP or name of your k3s server. kubectl can now manage your k3s cluster.

# Add a worker node

Get the token to access to the master 

```shell
sudo cat /var/lib/rancher/k3s/server/node-token
```

On the new RPI install l3s

```
wget https://github.com/rancher/k3s/releases/download/v0.2.0/k3s-armhf && \
  chmod +x k3s-armhf && \
  sudo mv k3s-armhf /usr/local/bin/k3s
```

```
export NODE_TOKEN="node-tokenhere"
export SERVER_IP="https://192.168.0.25:6443"
```

# Check you nodes

```shell
kubectl version
```

```
Client Version: version.Info{Major:"1", Minor:"13", GitVersion:"v1.13.5", GitCommit:"2166946f41b36dea2c4626f90a77706f426cdea2", GitTreeState:"clean", BuildDate:"2019-03-25T15:26:52Z", GoVersion:"go1.11.5", Compiler:"gc", Platform:"darwin/amd64"}
Server Version: version.Info{Major:"1", Minor:"13", GitVersion:"v1.13.4-k3s.1", GitCommit:"7f72ee72d6dce73e2d36dbd3402413d146fad01e", GitTreeState:"clean", BuildDate:"2019-03-04T01:05+00:00Z", GoVersion:"go1.11.4", Compiler:"gc", Platform:"linux/arm"}
```

```shell
kubectl get nodes
```

```
NAME    STATUS   ROLES    AGE   VERSION
derr    Ready    <none>   53m   v1.13.4-k3s.1
luxor   Ready    <none>   46s   v1.13.4-k3s.1
```

# Install Open FassS




# Install the OIPC-UA Appliction

One the cluster is ready we can install an application in teh cluster, you can get the deployment file [here](https://github.azc.ext.hp.com/galo/OPCUA-telemetry/blob/master/docker-compose.yml)

```shell
kubectl apply -f deplyment.yaml
```

# Install NodeExporter and Prometheus

https://blog.alexellis.io/prometheus-nodeexporter-rpi/
