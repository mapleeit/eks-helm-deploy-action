# EKS deployments with Helm

GitHub action for deploying to AWS EKS clusters using helm.

## Customizing

### inputs

Following inputs can be used as `step.with` keys

| Name             | Type    | Description                        |
|------------------|---------|------------------------------------|
| `aws-region`      | String  | AWS region to use. This must match the region your desired cluster lies in. |
| `cluster-name`      | String  | The name of the desired cluster. |
| `cluster-role-arn`      | String  | If you wish to assume an admin role, provide the role arn here to login as. |
| `config-files`      | String  | Comma separated list of helm values files. |
| `namespace`      | String  | Kubernetes namespace to use. |
| `values`      | String  | Comma separates list of value set for helms. e.x: key1=value1,key2=value2 |
| `name`      | String  | The name of the helm release |
| `chart-path`      | String  | The path to the chart. (defaults to `helm/`) |
| `timeout` | String | time to wait for any individual Kubernetes operation (like Jobs for hooks) (default to `'5m0s'`) |
| `install` | String | If a release by this name doesn't already exist, run an install. (defaults to `''`) |
| `wait` | String | if set, will wait until all Pods, PVCs, Services, and minimum number of Pods of a Deployment, StatefulSet, or ReplicaSet are in a ready state before marking the release as successful. It will wait for as long as timeout (defaults to `''`) |
| `debug` | String | enable verbose output (defaults to `''`) |


## Example usage

```yaml
jobs:
  helm-deploy:
    - name: Configure AWS Credentials
      uses: aws-actions/configure-aws-credentials@v1
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: us-west-2
    
    - name: Deploy via Helm
      uses: mapleeit/eks-helm-deploy-action@v1.3.0
      with:
        aws-region: us-west-2
        cluster-name: mycluster
        config-files: .github/values/dev.yaml
        chart-path: chart/
        namespace: dev
        values: key1=value1,key2=value2
        name: release_name
        install: 'true'
        wait: 'true'
        debug: 'true'
```