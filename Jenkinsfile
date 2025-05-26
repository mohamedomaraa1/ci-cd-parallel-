@Library('lab3')_

pipeline{
    agent {
        label 'agent-0'
    }

    tools{
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
                    org.session3.login(env.DOCKER_USER, env.DOCKER_PASS)
                }
            }
        }

        stage('Clone Repositories') {
            steps {
                script {
                    org.session3.gitClone('https://github.com/mohamedomaraa1/java.git', 'master', 'java')
                    org.session3.gitClone('https://github.com/mohamedomaraa1/pytohn1.git', 'main', 'python')
                }
            }
        }

        stage('Build and Push Docker Images') {
            parallel {
                stage('Java Image') {
                    steps {
                        dir('java') {
                            script {
                                org.session3.build('mohamedomaraa/java-app', 'latest')
                                org.session3.push('mohamedomaraa/java-app', 'latest')
                            }
                        }
                    }
                }

                stage('Python Image') {
                    steps {
                        dir('python') {
                            script {
                                org.session3.build('mohamedomaraa/python-app', 'latest')
                                org.session3.push('mohamedomaraa/python-app', 'latest')
                            }
                        }
                    }
                }
            }
        }
    }
}