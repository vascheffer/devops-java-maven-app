pipeline {

    agent any

    tools {
        maven 'maven-3.9'
    }

    stages {

        stage("build jar") {

            steps {
                script {
                    echo "building application with maven ... "
                    sh 'mvn package'
                }
            }
        }

        stage("build image") {

            steps {
                script {
                    echo "building docker image ... "
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-repo', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        sh 'docker build -t repository_name/demo-app:jma-1.0 .'
                        sh "echo $PASS | docker login -u $USER --password-stdin"
                        sh 'docker push repository_name/demo-app:jma-1.0'
                    }
                }
            }
        }

        stage("deploy") {

            steps {
                script {
                    echo "deploying... "
                }
            }
        }
    }
}