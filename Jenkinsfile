/**
 * This Jenkinsfile builds Theia across the major OS platforms
 */

pipeline {
    agent none
    environment {
        BUILD_TIMEOUT = 180
    }
    stages {
        stage('Installers') {
            parallel {
                stage('Create Linux Installer') {
                    agent {
                        kubernetes {
                            label 'node-pod'
                            yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: node
    image: node:10.17.0-buster-slim
"""
                    }
                    steps {
                        timeout(time: "${env.BUILD_TIMEOUT}") {
                            container('node') {
                                sh "printenv"
                                sh "yarn cache clean"
                                sh "yarn --frozen-lockfile --force"
                                sh "yarn package"
                            }
                        }
                    }
                }
                stage('Create Mac Installer') {
                    agent {
                        label 'mac'
                    }
                    steps {
                        timeout(time: "${env.BUILD_TIMEOUT}") {
                            sh "printenv"
                            sh "yarn cache clean"
                            sh "yarn --frozen-lockfile --force"
                            sh "yarn package"
                        }
                    }
                }
                stage('Create Windows Installer') {
                    agent {
                        label 'windows'
                    }
                    steps {
                        timeout(time: "${env.BUILD_TIMEOUT}") {
                            bat "set"
                            bat "yarn cache clean"
                            bat "yarn --frozen-lockfile --force"
                            bat "yarn package"
                        }
                    }
                }
            }
        }
    }
}