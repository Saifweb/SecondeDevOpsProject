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
        // test using maven
        // sh mean shell script 
        stage("Test the application"){
            steps {
                sh "mvn test"
            }
        }
        // sonaranalysis using maven cmd
       stage("SonarQube Analysis"){
           steps {
	           script {
                    withSonarQubeEnv(credentialsId: 'jenkins-sonarqube-token') { 
                            sh "mvn sonar:sonar"
                    }
	           }	
           }
       }
       // Wait for the quality gate and continue even if it fails
        stage("Quality Gate"){
           steps {
	           script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'jenkins-sonarqube-token'
	           }	
           }
       }
    }
}