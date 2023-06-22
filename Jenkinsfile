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

        stage('Deploy to Nexus') {
            steps {
               nexusArtifactUploader artifacts: [[artifactId: 'RegistrationApp',
               classifier: '', file: 'target/RegistrationApp-1.2.war',
               type: 'war']], 
               credentialsId: 'nexus', 
               groupId: 'com.example', 
               nexusUrl: '18.191.133.95:8081', 
               nexusVersion: 'nexus3', 
               protocol: 'http', 
               repository: 'project',
               version: '1.3'
            }
        }
        
        stage('Deploy to Tomcat') {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'tomcat', 
                path: '', 
                url: 'http://3.143.24.178:8080')],
                contextPath: 'mypath', 
                war: '**/*.war'
            }
        }
    }
}