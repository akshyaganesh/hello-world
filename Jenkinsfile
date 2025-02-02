pipeline{
    agent any
    environment {
        PATH = "$PATH:/opt/apache-maven-4.0.0-alpha-7/bin"
    }
    stages{
       stage('GetCode'){
            steps{
                // git 'https://github.com/akshyaganesh/hello-world.git'
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/akshyaganesh/hello-world.git']])
            }
       }        
       stage('Build'){
            steps{
                sh 'mvn clean package'
            }
       }
       stage('SonarQube analysis') {
            //    def scannerHome = tool 'SonarScanner 4.0';
            steps{
                withSonarQubeEnv('sonarqube-8.0') { 
                // If you have configured more than one global server connection, you can specify its name
                //      sh "${scannerHome}/bin/sonar-scanner"
                sh "mvn sonar:sonar"
                }
            }
       }
       stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t hello-world .'
                }
            }
       }

    }
}
