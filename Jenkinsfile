pipeline{
    agent {
    docker {
      image 'docker:stable'
      args '-v /var/run/docker.sock:/var/run/docker.sock'
    }
  }
    stages{
        stage('Build') {
      steps {
        sh 'docker build -t my-image .'
      }
    }
        stage("checkout"){
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'github',url: 'https://github.com/bhavymakadia/DockerPipeline']])
            }
        }
         stage("Build Image") {
            steps {
                script {
                    if (isUnix()) {
                        sh 'docker build -t bhavymakadia/dockerpipeline .'
                    } else {
                        bat 'docker build -t bhavymakadia/dockerpipeline .'
                    }
                }
            }
        }
         stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'DOCKERHUB_PASSWORD', usernameVariable: 'DOCKERHUB_USERNAME')]) {
                    script {
                        if (isUnix()) {
                            sh 'docker login -u $DOCKERHUB_USERNAME -p $DOCKERHUB_PASSWORD'
                            sh 'docker push bhavymakadia/dockerpipeline'
                            sh 'docker logout'
                        } else {
                            bat 'docker login -u %DOCKERHUB_USERNAME% -p %DOCKERHUB_PASSWORD%'
                            bat 'docker push bhavymakadia/dockerpipeline'
                            bat 'docker logout'
                        }
                    }
                }
            }
        }
    }
}