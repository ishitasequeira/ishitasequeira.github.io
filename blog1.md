Short term Goal for the Sprint
"We want to be able to run odo pipelines bootstrap with the correct parameters and be able to open a PR in GitHub and have it go through the pipelines."
 

 ODO command 

Below is the command that has been implemented by our team where we are specifying the deployment path, git repository, github token, image repository where the image needs to be stored and prefix. Git repository containing the application source code should be mentioned in "git-repo". Githubtoken is used to authenticate commit-status notifications to GitHub.The "image-repo" should either specify registry/username/repository to use Quay.io or Docker Hub or alternatively project/app to use the OpenShift internal image registry.Prefix helps to prevent the namespace conflict in the cluster as we have 3 fixed environments(cicd, dev, stage). We have applied this command to the cluster.

 ./odo pipelines bootstrap --deployment-path=deploy --dockerconfigjson=~/Downloads/cbanavik-auth.json --git-repo=chetan-rns/taxi --github-token=$TOKEN --image-repo=quay.io/cbanavik/taxi --prefix=sample | oc apply -f -

User Experience while running the odo command
After we execute the could command, the manifests get created. 3 environments would be created, that is the "cicd", "dev" and "stage" environment. For now, these are the fixed environments. All the pipelines are in the "cicd" environment. We have designed the "cicd" environment having 4 pipelines the "dev-ci-pipeline" , "dev-cd-pipeline", "stage-ci-pipeline" and the "stage-cd-pipeline".

The next step would be to add webhook to the git-repo. We would need a path for the webhook and that we could get that from evenlistener route that is mentioned in the cicd environment. We would need to run a command "oc get route" in the cicd environment.
 



As we create a pull request against the master of git-repo the "dev-ci-pipeline" gets triggered and then as soon as the PR is merged the "dev-cd-pipeline" gets triggered and the application is deployed in the dev environment.
In a similar way we could add the webhook to the stage repository.