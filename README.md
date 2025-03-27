# Crossplane2 Workspace

This repository contains configurations and resources for managing infrastructure using Crossplane. The structure of the repository is organized to facilitate the deployment and management of various cloud resources.

## Folder Structure

- **crossplane-notes**: Contains notes and documentation related to Crossplane usage and configurations.
- **1-providers-and-functions**: Contains YAML files for configuring providers and functions.
  - `aws-ec2.yaml`: Configuration for AWS EC2 provider.
  - `aws-eks.yaml`: Configuration for AWS EKS provider.
  - `aws-iam.yaml`: Configuration for AWS IAM provider.
  - `function-auto-ready.yaml`: Configuration for the auto-ready function.
  - `function-go-templating.yaml`: Configuration for the Go templating function.
  - `function-patch-and-transform.yaml`: Configuration for the patch and transform function.
  - `function-shell.yaml`: Configuration for the shell function.
  - `helm-and-kubernetes.yaml`: Configuration for Helm and Kubernetes providers.
- **2-providerconfig**: Contains YAML files for provider configurations.
  - `aws-providerconfig.yaml`: Configuration for AWS provider.
  - `helm-incluster-providerconfig.yaml`: Configuration for Helm in-cluster provider.
- **3-xrds**: Contains YAML files for Crossplane Composite Resource Definitions (XRDs).
  - `app-xrd.yaml`: XRD for application resources.
  - `eks-xrd.yaml`: XRD for EKS resources.
  - `vpc-xrd.yaml`: XRD for VPC resources.
- **4-compositions**: Contains YAML files for Crossplane compositions.
  - `app-composition.yaml`: Composition for application resources.
  - `eks-composition.yaml`: Composition for EKS resources.
  - `networking-composition.yaml`: Composition for networking resources.
- **5-resources**: Contains YAML files for resource claims and instances.
  - `vpc-claim.yaml`: Claim to create VPC and other networking components
  - `eks-claim.yaml`: Claim to create an EKS cluster, Nodegroups and addons
  - `app-claim.yaml`: Claim for deploy an application resources to the EKS cluster

## Usage

1. **Configure Providers**: Ensure that the provider configurations in the `2-providerconfig` folder are correctly set up with your cloud provider credentials.
2. **Deploy XRDs**: Apply the XRDs in the `3-xrds` folder to define the composite resources.
3. **Create Compositions**: Apply the compositions in the `4-compositions` folder to define how the composite resources should be composed.
4. **Claim Resources**: Use the resource claims in the `5-resources` folder to create instances of the composite resources.

```
helm repo add crossplane-stable https://charts.crossplane.io/stable
helm repo update
helm install crossplane \
--namespace crossplane-system \
--create-namespace crossplane-stable/crossplane
```

```
kubectl apply -f 1-providers-and-functions
kubectl apply -f 2-providerconfig
kubectl apply -f 3-xrds
kubectl apply -f 4-compositions
```
Wait for everything to be ready
```
kubectl apply -f 5-resources
```
watch the progress with the following commands:
```
watch "crossplane beta trace vpcclaims.aws.cloud.iac/crossplane-vpc"

watch "crossplane beta trace eksclaim.aws.cloud.iac/crossplane-eks-cluster"

watch "crossplane beta trace appclaim.aws.cloud.iac/game-2048"

watch "kubectl get managed"
```
### Deletion
Delete the claims first and wait for all resource to be destroyed before deleting other components like compositions, xrds, etc

## Acknowledgments

- [Crossplane](https://crossplane.io/) - The open-source project for managing cloud infrastructure.