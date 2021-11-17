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

Configuring GitHub
 

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

jenkins configuration for github

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
.
