#!groovy
pipeline {


    tools {nodejs "node"}

    environment {
        IMAGE_NAME = "zostaw/home-page"
        IMAGE_TAG = "python-app-1.0"
    }
    agent {
        kubernetes {
            // Rather than inline YAML, in a multibranch Pipeline you could use: yamlFile 'jenkins-pod.yaml'
            // Or, to avoid YAML:
            // containerTemplate {
            //     name 'shell'
            //     image 'ubuntu'
            //     command 'sleep'
            //     args 'infinity'
            // }
            yaml '''
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: shell
    image: python:3.10.5-alpine
    command:
    - sleep
    args:
    - infinity
'''
            // Can also wrap individual steps:
            // container('shell') {
            //     sh 'hostname'
            // }
            defaultContainer 'shell'
        }
    }

      stages {
        stage('Pre script') {
            steps {
                sh 'pwd'
                sh 'ls -las'
                sh 'apk --update add bash vim g++ gcc musl-dev linux-headers'
                sh 'pip install --upgrade pip'
                sh 'pip install --no-cache-dir -r ./requirements.txt'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Build') {
            agent{
                docker {
                    image "docker:20.10.21-alpine3.16"
                }
           }
            steps {
                echo 'Building..'
                sh "docker build -t $IMAGE_NAME:$IMAGE_TAG ."
                sh 'docker push $IMAGE_NAME:$IMAGE_TAG'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }

}
