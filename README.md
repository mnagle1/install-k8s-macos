# Install a k8s cluster on macos using vagrant and virtualbox

1. Download and [install Vagrant](https://www.vagrantup.com/downloads.html) (Latest Version: 2.2.4)
2. Download and [install VirtualBox](https://www.virtualbox.org/wiki/Downloads)
3. `cd workspace` on your mac 
4. `git clone https://github.com/mnagle1/install-k8s-macos.git`
5. `cd install-k8s-macos`
6. `vagrant up`
7. `vagrant global-status` returns the state of all 3 vms
```
id       name         provider   state   directory
-----------------------------------------------------------------------------------
7e09a58  k8s-master   virtualbox running /Users/mnagle/workspace/install-k8s-macos
7bdcde7  k8s-worker-1 virtualbox running /Users/mnagle/workspace/install-k8s-macos
2d4a88c  k8s-worker-2 virtualbox running /Users/mnagle/workspace/install-k8s-macos
```
8. `vagrant ssh k8s-master`
9. `kubectl get componentstatus` Now you can execute your kubectl commands..
```
vagrant@k8s-master:~$ kubectl get componentstatus
NAME                 STATUS    MESSAGE             ERROR
scheduler            Healthy   ok
controller-manager   Healthy   ok
etcd-0               Healthy   {"health":"true"}
```
10. If more than two nodes are required, you can edit the servers array in the /Users/mnagle/workspace/install-k8s-macos/Vagrantfile
```servers = [
   {
       :name => "k8s-worker-3",
       :type => "node",
       :box => "ubuntu/xenial64",
       :box_version => "20180831.0.0",
       :eth1 => "192.168.205.13",
       :mem => "2048",
       :cpu => "2"
   }
]
```
11. You can power down the vm's by running `vagrant suspend -a` (State goes from running to saved) and run vagrant up to bring them back up again (commands need to be run from the directory with Vagrantfile).
12. You can remove the k8s cluster by running `vagrant destroy -f`
