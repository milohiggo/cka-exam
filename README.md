# cka-exam
## Varous CKA resources for the exam

### Environment Shortcuts

        export k='kubectl' 
        #echo "ETCDCTL_API=3" >> $HOME/.profile
        export do='--dry-run=client -o yaml'  # note only used for create statement - otherwise use -o yaml
        #source $HOME/.bashrc

### Backup & resort etcd
       Install etcdctl client:  sudo apt install etcd-client - [GITHUB Link](https://github.com/etcd-io/etcd/issues/14190)
       
       Manifest for etcd are in /etc/kubernetes/manifest/etcd.yaml    --> also show the data directory usually /var/lib/etcd

        First setup variable:
                export ETCDCTL_API=3
                export ETCDCTL_CACERT=/etc/kubernetes/pki/etcd/ca.crt 
                export ETCDCTL_CERT=/etc/kubernetes/pki/etcd/server.crt 
                export ETCDCTL_KEY=/etc/kubernetes/pki/etcd/server.key
        Second setup short cut:
                alias myetcd="sudo ETCDCTL_API=$ETCDCTL_API ETCDCTL_CACERT=$ETCDCTL_CACERT ETCDCTL_CERT=$ETCDCTL_CERT ETCDCTL_KEY=$ETCDCTL_KEY etcdctl"

        To see health : sudo ETCDCTL_API=$ETCDCTL_API ETCDCTL_CACERT=$ETCDCTL_CACERT ETCDCTL_CERT=$ETCDCTL_CERT ETCDCTL_KEY=$ETCDCTL_KEY etcdctl endpoint health
                   or   myetcd endpoint health

        To count DB in cluster : sudo ETCDCTL_API=$ETCDCTL_API ETCDCTL_CACERT=$ETCDCTL_CACERT ETCDCTL_CERT=$ETCDCTL_CERT ETCDCTL_KEY=$ETCDCTL_KEY etcdctl --endpoints=https://127.0.0.1:2379 member list -w table
                   or  myetcd --endpoints=https://127.0.0.1:2379 member list -w table

        Create Snapshot:

        sudo etcdctl snapshot save <file> --cacert=$CACERT --cert=$CERT --key=$KEY
        example: --endpoints=https://127.0.0.1:2379 snapshot save /var/lib/etcd/snapshot.db

        Now lets create a backup Vault to store this in:
        mkdir $HOME/backup
        sudo cp /var/lib/etcd/snapshot.db $HOME/backup/snapshot.db-$(date +%m-%d-%y)
        sudo cp /root/kubeadm-config.yaml $HOME/backup/
        sudo cp -r /etc/kubernetes/pki/etcd $HOME/backup/
        
        sudo systemctl stop etcd
        sudo etcdctl snapshot restore <file> --data-dir <directory> 
        sudo systemctl start etcd
        sudo systemctl daemon-reload

### Upgrade kubeadm, kubectl and kubelet on a worker
        kubeadm upgrade apply (control plane)
        kubeadm upgrade node (worker node)
        apt-get install -y --allow-change-held-packages kubeadm=1.20.1–00 kubectl=1.20.1–00 kubelet=1.20.1–00

### Fix broken worker
        sudo systemctl status kubelet
        sudo systemctl enable kubelet
        sudo systemctl start kubelet
        sudo systemctl daemon-reload

### Use journalctl
        sudo journalctl -u <service-name>
        Press Shift+G to scroll to the bottom of the log where the most recent event occurred

## CKA Resource Links

https://github.com/reachcloudsme/k8s-certifications-resources/blob/main/CKA.md

https://github.com/reachcloudsme/k8s-certifications-resources/blob/main/Prereqs.md




