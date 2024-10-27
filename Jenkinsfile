pipeline {
    agent { label 'ubuntu' }

    tools {
        maven 'Apache Maven 3.8.7'  // Use the name defined in Global Tool Configuration
    }

    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }
        stage('Source Code Checkout') {
            steps {
                checkout changelog: false, poll: false, scm: scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'github_cred', url: 'https://github.com/DevOps24-Ashutosh/Semaphore_Java_SpringBoot.git']])
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Unit Tests') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Integration Test') {
            steps {
                sh 'mvn test -Pintegration-testing'
            }
        }
        stage('Performance Tests') {
            steps {
                sh 'mvn jmeter:jmeter'
            }
        }
        stage('Build Docker Image') {
            environment {
                imageTag = "${BUILD_NUMBER}"
            }
            steps {
                sh 'docker build -t ashuto91/semaphore:${imageTag} .'
            }
        }
        stage('Push Docker Image to Docker Hub') {
            environment {
                registryCredential = 'dockerHubCred'
            }
            steps {
                script {
                    docker.withRegistry('', registryCredential) {
                        sh 'docker push ashuto91/semaphore:${BUILD_NUMBER}'
                    }
                }
            }
        }
        // stage("Removing the previous Build Image") {
        //     steps {
        //         sh '''
        //         echo "Removing the previous built image"
        //         docker rmi ashuto91/semaphore:${BUILD_NUMBER}
        //         '''
        //     }
        // }
        // stage('Source Code Checkout') {
        //     steps {
        //         echo 'Hello World'
        //     }
        // }
    }
}
