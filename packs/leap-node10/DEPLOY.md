
# Deploy to Production
Applications are deployed to the serverless framework Knative to manage zero to infinity autoscaling in a controlled environment.
You can disable Knative and use regular helm deployments if you choose by setting `knativeDeploy` to `false` in `charts/<your-repo-name>/values.yaml`.
The only drawback of using Knative with the current architecture is that it requires manual deployments. In either case the jenkins CI and previews will still be automated.

## Deploy
### Manual
If you are using non-knative deployments you will only need to do a manual deployment after merging in changes that modify the contents of `charts/<your-repo-name>/`. Otherwise if you are using Knative you will have to run this redeployment for every release
1. Wait for the jenkins x pipeline to finish (check logs with `jx get build log -f <your-repo-name>`).
2. Connect to the kubernetes cluster you want to deploy to
3. `helm repo update`
4. `helm update -n <your-namespace> <your-service-name> leap/<your-repo-name>`

#### Substitute these steps for initial deployment
3. `helm repo add leap https://chartmuseum-jx.123leap.ca`
4. `helm install -n <your-namespace> <your-service-name> leap/<your-repo-name>`

### Rollback
#### Non Knative
The docker image with the `latest` tag is pulled into prod on demand. Therefore to do a rollback you just have to update the `latest` tag in GCR to the version you want to rollback to. The next deployment will automatically update the `latest` tag so no additional work should be needed. If you did a manual deployment of the helm chart you will also have to run the rollback outlined in the Knative section below.
#### Knative
To revert you have to do a `helm rollback` with the chart version you want to rollback.
