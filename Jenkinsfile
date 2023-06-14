pipeline 
{
    agent any

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/leoimewore/testrepo'
            }
        }
        
        stage("SonarQube analysis") {
           steps {
             withSonarQubeEnv('sonar') {
                 sh 'mvn clean package sonar:sonar'
             }
           }
        }
        
        stage('Quality Gate') {
            steps {
                waitForQualityGate abortPipeline: true
            }
        }
        
        stage('Unit Test') {
            steps {
                sh 'mvn test'
            }
        }
        
        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }
        
        stage('Deploy to Tomcat') {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'tomcat', 
                path: '', 
                url: 'http://3.17.79.127:8080')],
                contextPath: 'mypath', 
                war: '**/*.war'
            }
        }
    }
}