@Library('lab3')_
def dockerx = new org.session3.docker()

pipeline {
    agent {
        label 'agent-0'
    }

    tools {
        jdk "java-21" // Update to match OpenJDK 21, or configure "java-8" in Jenkins to point to the correct JDK
    }

    environment {
        DOCKER_USER = credentials('docker-user')
        DOCKER_PASS = credentials('docker-pass')
        JAVA_HOME = "/usr/lib/jvm/java-21-openjdk-amd64" // Adjust based on your agent's JDK installation path
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
                        dir('java') {
                            script {
                                // Ensure JAVA_HOME is set for Maven commands
                                withEnv(["JAVA_HOME=${env.JAVA_HOME}", "PATH+MAVEN=${env.JAVA_HOME}/bin:${env.PATH}"]) {
                                    dockerx.buildJava()
                                    dockerx.push('mohamedomaraa/java-app', 'latest')
                                }
                            }
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
