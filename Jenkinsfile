pipeline {
  agent {
    label 'worker_node1'
  }


    stages {
        stage('Checkout') {
            steps {
                // Get some code from a GitHub repository
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/huicodes/maven-project.git']]])

            }
        }
          
        stage('Build') {
            steps {
            
                // Run Maven on a Unix agent.
                sh "mvn clean package"

            }
        }
              
        stage('Build Docker Image') {
            steps {
            
                // create docker build
                sh "docker build -t webapp:v${BUILD_NUMBER} ."

            }
        }
          
      stage('Push image to Docker Hub') {
          steps {
              sh "docker tag webapp:v${BUILD_NUMBER}  henike94/webapp:v${BUILD_NUMBER}"
              sh "docker push henike94/webapp:v${BUILD_NUMBER}"
            }
        }
        
      stage('Runthe webapp container') {
          steps {
              sh "docker run --name webapp_v${BUILD_NUMBER} -p 8087:8080 -d henike94/webapp:v${BUILD_NUMBER}"
            }
        }
    }
}
