#### DevOps Lifecycle Stages
- Continuous Development
- Continuous Testing
- Continuous Integration
  - CI is the practice of merging all developers working copies to a shared mainline several times a day
    - Development
    - Testing
    - Integration
- Continuous Delivery/Deployment
  - CDE (continuous delivery) is a software engineering approach in which teams produce software in short cycles, ensuring that the software can be reliably released at any time and, when releasing the software, doing so manually
  - CD (Continuous Deployment ) is a software release process that uses automated testing to validate if changes to a codebase are correct and stable for immediate autonomous deployment to a production environment.
- Continous Operations

#### Why CI/CD matters
- Reduce costs
- Faster release rate
- Smaller code changes
- Fault isolations
- More test reliabilty
- Increase team transparency and accountability
- Easy maintenance and updates

#### Generate new ssh keys
ssh-keygen -t rsa -b 4096 -C "abs@githubemail.com"

reference for complete basic github jenkins integration: https://www.blazemeter.com/blog/how-to-integrate-your-github-repository-to-your-jenkins-project

#### Configuring GitHub
 
Step 1: go to your GitHub repository and click on ‘Settings’.

integrating jenkins and github

Step 2: Click on Webhooks and then click on ‘Add webhook’.

add github to your ci

Step 3: In the ‘Payload URL’ field, paste your Jenkins environment URL. At the end of this URL add /github-webhook/. In the ‘Content type’ select: ‘application/json’ and leave the ‘Secret’ field empty.

jenkins and gihutb integration tutorial

Step 4: In the page ‘Which events would you like to trigger this webhook?’ choose ‘Let me select individual events.’ Then, check ‘Pull Requests’ and ‘Pushes’. At the end of this option, make sure that the ‘Active’ option is checked and click on ‘Add webhook’.

jenkins and github integration guide

configure github for CI integration
We're done with the configuration on GitHub’s side! Now let's move on to Jenkins.

Configuring Jenkins

Step 5: In Jenkins, click on ‘New Item’ to create a new project.

#### jenkins configuration for github

Step 6: Give your project a name, then choose ‘Freestyle project’ and finally, click on ‘OK’.

jenkins ci, github scm

Step 7: Click on the ‘Source Code Management’ tab.

adding your scm to continuous integration

Step 8: Click on Git and paste your GitHub repository URL in the ‘Repository URL’ field.

GitHub and Jenkins combination

Step 9: Click on the ‘Build Triggers’ tab and then on the ‘GitHub hook trigger for GITScm polling’. Or, choose the trigger of your choice.

Triggering the Jenkins Job to Run with Every Code Commit

Step 10: Click on the ‘Build’ tab, then click on ‘Add build step’ and choose ‘Execute shell’.

Step 11: To run a Tests etc cd app, npm install, npm test.

Step 12: Go back to your GitHub repository, edit the readme file and commit the changes. We will now see how Jenkins ran the script after the commit.

Step 13: Go back to your Jenkins project and you'll see that a new job was triggered automatically from the commit we made at the previous step. Click on the little arrow next to the job and choose ‘Console Output’

Step 14: You can see that Jenkins was able to pull the test and run it!

pull a script from github and run it
Congratulations! Every time you publish your changes to Github, GitHub will trigger your new Jenkins job.

### Jenkins

- Jenkins is an open-source continuous integration tool. Being open-source, it is extremely popular and widely used

- As it reduces the manual steps required to get from writing software to it beign deployedgreatly, software like Jenkins is an essential part of building a DevOps culture

- Huge range of plugins are available, including community-created ones that suit almost any purpose.

- Jenkins can be aimed at a git repository, and will then

- Jenkins is used to set up pipelines, which run in a series of stages to build a sample of recently pushed changes

- If all tests are passed, these can then be passed on, for example to a dockerhub repository, where they are then ready for deployment

- It is also possible to extend the scope of Jenkins all the way to deployment, or simply to run tests periodically.

- Highly adaptable, and the fact that it is used by a wide variety of huge companies shows that it can be used ot get hte job done.

Development computer has ssh key available, and also a deploy key for git. Jenkins listens to the github and keeps track of any new pushes to the github.

So, need to generate another key that allows jenkins to take the github code

- Good practice is to operate tests on an agent node, which is spun up by the master node. This prevents a bad test from breaking the entire Jenkins server
```
The basic process will be:

A developer (in this case, us), adds or changes the code

The code is pushed to the github repository

Jenkins notices that a new commit has been pushed to the repo and starts a pipeline, authenticating with the deploy key
```
To do this, need to set up a webhook:

- Firstly, the settings in jenkins should be to use github

- after that, it is as simple as setting up the webhook in the github settings for the repo

  - you can see the webhook in the settings on this repo (though the instance may be spun down by now
  - The URL to give is the jenkins server's, including port number. Then, add ```/github-webhook/``` to complete the address

Basically, github will send a POST request to that location, triggering Jenkins to clone the repo and run a build.

So now that this has been tested, a better practice would be to have Jenkins run tests and only merge the changes into main once they are passed.

To do this, this repo will gain a branch called ```dev```, which we will get Jenkins to listen to.

We can then create a new job to merge the branches.

```This code should be pushed to dev, but merged into main by Jenkins```

#### Deploy Jobs in Jenkins
```
# we need to by pass the key asking stage with below command:
ssh -A -o "StrictHostKeyChecking=no" ubuntu@ec2-ip << EOF	
# copy the the code
# run your provision.sh to install node with required dependencies for app instance - same goes for db instance (ensure to double check if node and db are actively running)

# create an env to connect to db
# navigate to app folder
# kill any existing pm2 process just in case
# launch the app
nohup node app.js > /dev/null 2>&1 & - use this command to run node app in the background

# To debug ssh into your ec2 and run the above commands
    
EOF
```
#### Jenkins commands for running aws
```
ssh -A -o "devopsbootcamp.pem" ubuntu@ec2-18-202-31-157.eu-west-1.compute.amazonaws.com
sudo apt-get update
sudo apt-get upgrade
scp -i app ubuntu@ec2-18-202-31-157.eu-west-1.compute.amazonaws.com:~/home/ubuntu/app
cd app
npm install
npm test
```

#### Jenkins CI Lab - Solution
Steps

Source Code Management
- Set Branches to Build to develop
- Under additional behaviours click add and "Merge before build"
- name of repo "origin"
- branch to merge "main"

Post-Build Actions
- Git Publisher
- Add Post Build Action
- Git Publisher
- Push Only if Build Succeeds
- Merge Results

Tigger deployment job if the merge was successfull

