EKS cluster provisioning demo
 OR 
 You can also use ekstcl 

1. create an eksconfig file, name it ha-cluster-config.yaml and paste the 


 apiVersion: eksctl.io/v1alpha5
kind: ClusterConfig

metadata:
  name: ha-cluster
  region: us-west-2

# VPC configuration, you can customize this as needed
vpc:
  cidr: "10.0.0.0/16"
  subnets:
    private:
      us-west-2a: { cidr: "10.0.1.0/24" }
      us-west-2b: { cidr: "10.0.2.0/24" }
      us-west-2c: { cidr: "10.0.3.0/24" }
    public:
      us-west-2a: { cidr: "10.0.4.0/24" }
      us-west-2b: { cidr: "10.0.5.0/24" }
      us-west-2c: { cidr: "10.0.6.0/24" }

# Control plane configuration
managedNodeGroups:
  - name: worker-nodes
    instanceType: t3.medium
    desiredCapacity: 3
    minSize: 3
    maxSize: 6
    availabilityZones: ["us-west-2a", "us-west-2b", "us-west-2c"]

iam:
  withOIDC: true

cloudWatch:
  clusterLogging:
    enableTypes: ["audit", "api", "authenticator"]

2. use the following to have your cluster running 

-eksctl create cluster -f ha-cluster-config.yaml