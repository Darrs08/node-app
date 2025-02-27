pipeline {
    agent any
    environment {
        DOCKER_TAG = getDockerTag()
    }
    stages{
        stage('Build Docker Image') {
            steps {
                sh "docker build . -t darrs08/nodeapp:${DOCKER_TAG}"
            }
        }
        stage('DokerHub Push') {
            steps{
                withCredentials([string(credentialsId: 'docker-hub', variable: 'dockerHubPwd')]) {
                    sh "docker login -u darrs08 -p ${dockerHubPwd}"
                    sh "docker push darrs08/nodeapp:${DOCKER_TAG}"
                }
            }
        }
       stage('Deploying App to Kubernetes') {
           steps {
               script {
                   kubernetesDeploy(configs: "pods.yml", kubeconfigId: "kubernetes")
               }
           }
      }
    }
}

def getDockerTag(){
    def tag = sh script: 'git rev-parse HEAD', returnStdout: true
    return tag
}
