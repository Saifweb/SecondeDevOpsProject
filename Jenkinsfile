pipeline{
    agent{
        label "Jenkins-slave"
    }
    tools {
        jdk 'java17'
        maven 'maven3'
    }
    stages{
        // ensures that there are no residual artifacts or files from previous build
        stage("Cleanup Workspace"){
            steps{
                cleanWs()
            }
        }
        //get the code from github repo!!
        stage("Checkout from the SCM"){
            steps{
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/Saifweb/SecondeDevOpsProject.git'
                }
        }
    }
}