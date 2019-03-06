# k3s-vagrant
Vagrantを用いてKubernetes環境（k3s）を簡単に作成します。
このブランチは余計なものが一切入っていない最小限の状態で起動します。

# 前提
以下は事前に導入してあるものとします。
- VirtualBox
- Vagrant
- git

動作はWindwos 10で確認していますが、macOSでも同様に動作すると思われます。

# 導入
以下を実行すると、CentOS 7がk3sが導入された状態で起動します。
```
git clone https://github.com/k8sinfo/k3s-vagrant.git && cd k3s-vagrant && vagrant up && vagrant ssh
```

# 動作確認
```
[vagrant@localhost ~]$ kubectl get node
NAME                    STATUS   ROLES    AGE   VERSION
localhost.localdomain   Ready    <none>   36s   v1.13.3-k3s.6

[vagrant@localhost ~]$ kubectl get pod -n kube-system
NAME                             READY   STATUS              RESTARTS   AGE
coredns-7748f7f6df-c6kkc         1/1     Running             0          34s
helm-install-traefik-xfj44       0/1     Completed           0          34s
svclb-traefik-5757cdf7cf-886z5   0/2     ContainerCreating   0          2s
traefik-dcd66ffd7-4gghc          0/1     ContainerCreating   0          2s

[vagrant@localhost ~]$ kubectl get ns
NAME          STATUS   AGE
default       Active   111s
kube-public   Active   111s
kube-system   Active   111s
```