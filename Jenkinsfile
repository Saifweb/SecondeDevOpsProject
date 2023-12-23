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
        // build using maven
        // sh mean shell script 
        stage("Build the application"){
            steps {
                sh "mvn clean package"
            }
        }
        // build using maven
        // sh mean shell script 
        stage("Test the application"){
            steps {
                sh "mvn test"
            }
        }
    }
}