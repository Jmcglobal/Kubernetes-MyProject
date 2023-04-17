
## Using File-System Volume instead of EBS

- First configure efs file system on AWS

- Get the file system ID and enter it on volume handle mode 

- Deploy efs-csi-driver inside the kubernetes cluster

        kubectl apply -k "github.com/kubernetes-sigs/aws-efs-csi-driver/deploy/kubernetes/overlays/stable/?ref=release-1.5"
  
  Using the efs.yaml file. apply the configuration inside the cluster.
