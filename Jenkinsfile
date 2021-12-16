pipeline {
    agent {
        node {
            // https://jenkins.test/computer/(built-in)/configure
            label 'docker-enabled'
        }
    }

    environment {
        // https://hub.docker.com/repositories
        imageName = "aritzcas/prueba"

        // https://hub.docker.com/settings/security
        registryCredential = 'docker-hub'

        publicPort = "9005"
    }

    stages {
        stage('Test') {
            steps {
                sh 'echo Fake test'
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    dockerImage = docker.build(imageName, "-f Dockerfile .")
                }
            }
        }

        stage('Docker Push') {
            steps {
                script {
                    // Primer parámetro en blanco -> Docker Hub, si no, URL del registro privado
                    docker.withRegistry('', registryCredential) {
                        dockerImage.push('latest')
                    }
                }
            }
        }

        stage('Docker Run') {
            steps {
                sh "docker rm -f nginx-aritz"
                sh "docker run -d -p ${publicPort}:80 --name nginx-aritz ${imageName}:latest"
            }
        }
    }
}
