# cka-exam
## Varous CKA resources for the exam

### Environment Shortcuts

        export k='kubectl' 
        #echo "ETCDCTL_API=3" >> $HOME/.profile
        export do='--dry-run=client -o yaml'
        #source $HOME/.bashrc

### Backup & resort etcd
        sudo etcdctl snapshot save <file> --cacert=$CACERT --cert=$CERT --key=$KEY
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



