pipeline {
    agent any

    stages {

        stage('Git') {
            steps {
                cleanWs()
                git branch: 'master', url: 'https://github.com/e-Learning-by-SSE/infrastructure-piston-service.git'
            }
        }

        stage('Docker') {
            steps {
                sh 'cd api'
                script {
                    env.API_VERSION = '0.1'
                    echo "API: ${env.API_VERSION}"
                    dockerImage = docker.build 'e-learning-by-sse/infrastructure-piston'
                    docker.withRegistry('https://ghcr.io', 'github-ssejenkins') {
                        dockerImage.push("${env.API_VERSION}")
                        dockerImage.push('latest')
                    }
                }
            }
        }
    }
}
