# k3s-vagrant
Vagrantを用いてKubernetes環境（k3s）を簡単に作成します。

# 前提
以下は事前に導入してあるものとします。
- VirtualBox
- Vagrant
- git

動作はWindwos 10で確認していますが、macOSでも同様に動作すると思われます。

# 導入
以下を実行すると、k3sが導入されたVMが3インスタンス起動します。
```
git clone https://github.com/k8sinfo/k3s-vagrant.git && cd k3s-vagrant && vagrant up && vagrant ssh master
```

2回目以降の起動は`vagrant up`だけで起動します。

# 停止
ホストOSのVagrantfileのあるディレクトリで`vagrant halt`を実行します。

# 仕様

- OSはCentOS 7です。

- 各ノードの役割は以下の通り。
    - k3s-master : k8sでいうMasterの役割。
    - k3s-node01, k3s-node02 : k8sでいうNode(Worker)の役割。

# 動作確認
```
[vagrant@master ~]$ kubectl get node
NAME         STATUS   ROLES    AGE   VERSION
master.k3s   Ready    <none>   26m   v1.13.3-k3s.6
node01.k3s   Ready    <none>   54s   v1.13.3-k3s.6
node02.k3s   Ready    <none>   29s   v1.13.3-k3s.6

[vagrant@master ~]$ kubectl get po -o wide -n kube-system
NAME                             READY   STATUS      RESTARTS   AGE   IP          NODE         NOMINATED NODE   READINESS GATES
coredns-7748f7f6df-cl2cn         1/1     Running     1          27m   10.42.0.7   master.k3s   <none>           <none>
helm-install-traefik-8vncb       0/1     Completed   0          27m   10.42.0.3   master.k3s   <none>           <none>
svclb-traefik-57d4d8ff7f-vht8h   2/2     Running     2          26m   10.42.0.6   master.k3s   <none>           <none>
traefik-6876857645-zql5b         1/1     Running     1          26m   10.42.0.8   master.k3s   <none>           <none>

[vagrant@localhost ~]$ kubectl get ns
NAME          STATUS   AGE
default       Active   111s
kube-public   Active   111s
kube-system   Active   111s
```