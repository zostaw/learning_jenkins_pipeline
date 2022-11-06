#!groovy
pipeline {


    tools {nodejs "node"}

    environment {
        IMAGE_NAME = "zostaw/numpy"
        IMAGE_TAG = "python-numpy-1.0"
        dockerhub = credentials("dockerhub")
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
    image: docker:20.10.21-alpine3.16
    command:
    - sleep
    args:
    - infinity
    tty: true
    volumeMounts:
      - mountPath: /var/run/docker.sock
        name: docker-sock
  volumes:
  - name: docker-sock
    hostPath:
      path: /var/run/docker.sock
'''
            // Can also wrap individual steps:
            // container('shell') {
            //     sh 'hostname'
            // }
            defaultContainer 'shell'
        }
    }

      stages {
        stage('Build docker image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
            }
        }
        stage('Deploy to dockerhub') {
            steps {
                echo 'Deploying....'
                sh 'docker login -u $dockerhub_USR -p $dockerhub_PWD'
                sh 'docker image push $IMAGE_NAME:$IMAGE_TAG'
            }
        }
    }

}
