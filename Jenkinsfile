pipeline {
    agent any 
    tools {
        jdk 'JDK_17'
        nodejs 'node16'
    }
    environment {
        SCANNER_HOME=tools 'sonar-scanner'
    }
    stages {
        stage('clean workspace') {
            steps {
                cleanWs()
            }
        }
        stage('checkout from Git') {
            steps {
                git url: 'https://github.com/Hitansu26/Zomato-Clone.git',       
                
                    branch: 'main'
            }
        }
         stage("Sonarqube Analysis "){
            steps{
                withSonarQubeEnv('sonar-server') {
                    sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Zomato-clone \
                    -Dsonar.projectKey=zomato '''
                }
            }
        }
           stage("quality gate"){
           steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonat-token' 
                }
            } 
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
    }
}