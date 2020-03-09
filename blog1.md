

This week we completed only our second sprint, the goal for this sprint was:

    "We want to be able to run odo pipelines bootstrap with the correct parameters and be able to open a PR in GitHub and have it go through the pipelines."

Our goal is to make it easy to bootstrap an environment that can automatically deploy changes from GitHub.

To do this, we are extending OpenShift's Odo

    ./odo pipelines bootstrap --deployment-path=deploy --dockerconfigjson=~/Downloads/cbanavik-auth.json --git-repo=chetan-rns/taxi --github-token=$TOKEN --image-repo=quay.io/cbanavik/taxi --prefix=sample | oc apply -f -

TODO remove the references to Chetan in the command above.

TODO reword this to explain what the command line options do...you can paraphrase e.g.

    The --git-repo option provides a Git repository that we can react to changes for, pull requests trigger CI runs, merges to master trigger deployments.

Below is the command that has been implemented by our team where we are specifying the deployment path, git repository, github token, image repository where the image needs to be stored and prefix. Git repository containing the application source code should be mentioned in "git-repo". Githubtoken is used to authenticate commit-status notifications to GitHub.The "image-repo" should either specify registry/username/repository to use Quay.io or Docker Hub or alternatively project/app to use the OpenShift internal image registry.Prefix helps to prevent the namespace conflict in the cluster as we have 3 fixed environments(cicd, dev, stage).

When we apply the output from this command, three environments are created

    "cicd" TODO explain what we use each environment for?
    "dev"
    "stage"

We use four pipelines to automate deployments to the "dev" and "stage" environments:

    dev-ci-pipeline TODO put a short explanation of what the pipeline does
    dev-cd-pipeline
    stage-ci-pipeline
    stage-cd-pipeline

When we add GitHub webhooks pointing to our cluster, this means we can drive our process from GitHub.

As we create a pull request against our application source code, the "dev-ci-pipeline" gets triggered and then as soon as the PR is merged the "dev-cd-pipeline" gets triggered and the application is deployed in the dev environment.

In a similar way we could add the webhook to the stage repository, and merges to the stage environment's configuration would be automatically deployed.
