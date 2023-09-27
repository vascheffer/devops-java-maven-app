#!/user/bin/env groovy
// @Library('jenkins-shared-library') // this is for global use

library identifier: 'jenkins-shared-library@master', retriever: modernSCM(
    [$class: 'GitSCMSource',
    remote: 'https://github.com/vascheffer/devops-jenkins-shared-library.git',
    credentialsId: 'github-credentials'])

def gv

pipeline {

    agent any

    tools {
        maven 'maven-3.9'
    }

    stages {

        stage("init") {
            steps {
                script {
                    gv = load "script.groovy"
                }
            }
        }

        stage("build jar") {

            steps {
                script {
                    // gv.buildJar()
                    buildJar()
                }
            }
        }

        stage("build and push image") {

            steps {
                script {
                    //gv.buildImage()
                    buildImage 'vascheffer/demo-app:jma-2.0'
                    dockerLogin()
                    dockerPush 'vascheffer/demo-app:jma-2.0'
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