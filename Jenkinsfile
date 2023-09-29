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

        stage('increment version') {
            steps {
                script {
                    echo 'incrementing app version...'
                    sh 'mvn build-helper:parse-version versions:set \
                        -DnewVersion=\\\${parsedVersion.majorVersion}.\\\${parsedVersion.minorVersion}.\\\${parsedVersion.nextIncrementalVersion} \
                        versions:commit'
                    def matcher = readFile('pom.xml') =~ '<version>(.+)</version>'
                    def version = matcher[0][1]
                    env.IMAGE_NAME = "$version-${env.BUILD_NUMBER}" // build number is generate by jenkins
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
                    buildImage "vascheffer/demo-app:${env.IMAGE_NAME}"
                    dockerLogin()
                    dockerPush "vascheffer/demo-app:${env.IMAGE_NAME}"
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