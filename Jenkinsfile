@Library('lab3')_
def dockerx = new org.session3.docker()

pipeline {
    agent {
        label 'agent-0'
    }

    tools {
        jdk "java-8"
    }

    environment {
        DOCKER_USER = credentials('docker-user')
        DOCKER_PASS = credentials('docker-pass')
    }

    stages {
        stage('Login to DockerHub') {
            steps {
                script {
                    dockerx.login(env.DOCKER_USER, env.DOCKER_PASS)
                }
            }
        }

        stage('Clone Repositories') {
            steps {
                script {
                    dockerx.gitClone('https://github.com/mohamedomaraa1/java.git', 'master', 'java')
                    dockerx.gitClone('https://github.com/mohamedomaraa1/pytohn1.git', 'main', 'python')
                }
            }
        }

        stage('Build and Push Docker Images') {
            parallel {
                stage('Java Image') {
                    steps {
                        script {
                            dockerx.buildJava()
                            dockerx.push('mohamedomaraa/java-app', 'latest')
                        }
                    }
                }

                stage('Python Image') {
                    steps {
                        dir('python') {
                            script {
                                dockerx.build('mohamedomaraa/python-app', 'latest')
                                dockerx.push('mohamedomaraa/python-app', 'latest')
                            }
                        }
                    }
                }
            }
        }
    }
}
