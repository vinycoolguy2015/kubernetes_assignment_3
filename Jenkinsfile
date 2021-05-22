pipeline {
    agent any
    triggers {
        githubPush()
      }
    environment {
        DOCKER_IMAGE_NAME = "vinycoolguy/guestbook"
    }
    stages {
        
        stage('Build Docker Image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    app = docker.build(DOCKER_IMAGE_NAME)
                    
                }
            }
        }
        stage('Push Docker Image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_login') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }
        
        stage('Deployment') {
            when {
                branch 'master'
            }
            steps {
                sh '/usr/local/bin/helm upgrade --install guestbook ./guestbook-deployment --namespace development --set image.tag="${BUILD_NUMBER}"'
            }
        }
       
    }
}
