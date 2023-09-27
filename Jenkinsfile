#!/user/bin/env groovy
@Library('jenkins-shared-library')

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

        stage("build image") {

            steps {
                script {
                    //gv.buildImage()
                    buildImage()
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