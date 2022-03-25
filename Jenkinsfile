#!/usr/bin/env groovy

library identifier: 'jenkins-shared-library@master', retrieval: modernSCM(
    [$class: 'GitSCMSource',
    remote: 'https://github.com/aleqxan/jenkins-CICD.git'
    credentialsId: 'github-credentials'
    ]
)
def gv

pipeline {
    agent any
    tools {
        maven 'Maven'
    }
    stages {
        stage("init") {
            steps {
                script {
                    gv = load "script.groovy"
                }
            }
        }
        stage("buildjar") {
            steps {
                script {
                    buildJar()
                }
            }
        }
        stage("build and push image") {
            steps {
                script {
                    buildimage 'Javaapp/jar-app:jma-3.0'
                    dockerLogin()
                    dockerPush 'Javaapp/jar-app:jma-3.0'
                }
            }
        }
        stage("deploy") {
            steps {
                script {
                    gv.deployApp()
                }
            }
        }
    }
}