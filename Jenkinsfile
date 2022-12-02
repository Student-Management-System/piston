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
                script {
                    dir('api') {
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
        
        stage('Deploy') {
            steps {
                sshagent(credentials: ['Stu-Mgmt_Demo-System']) {
                    sh """
                        # [ -d ~/.ssh ] || mkdir ~/.ssh && chmod 0700 ~/.ssh
                        # ssh-keyscan -t rsa,dsa example.com >> ~/.ssh/known_hosts
                        ssh -i ~/.ssh/id_rsa_student_mgmt_backend elscha@${env.DEMO_SERVER} <<EOF
                            cd /staging/nm-self-learning
                            ./recreate.sh
                            exit
                        EOF"""
                }
            }
        }
        
        stage('Cleanup') {
            steps {
                sh 'docker image prune -f'
            }
        }
    }
}
