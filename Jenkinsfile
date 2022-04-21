pipeline {
  agent {
     kubernetes {
      defaultContainer 'jenkins-slave'
      yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
    name: test-sdk
spec:
  volumes:
    - name: docker-sock
      hostPath:
        path: /var/run/docker.sock
  containers:
  - name: glams-jenkins-slave
    image: anandsadhu/myapp:latest
    imagePullPolicy: Always
    command:
    - cat
    tty: true
    resources:
      requests:
        memory: "2Gi"
    volumeMounts:
     - name: docker-sock
       mountPath: /var/run/docker.sock
"""
    }
}

triggers {
  pollSCM('H/2 * * * *')
}

options {
  buildDiscarder(logRotator(numToKeepStr: '10'))
  skipStagesAfterUnstable()
  durabilityHint('PERFORMANCE_OPTIMIZED')
  disableConcurrentBuilds()
  skipDefaultCheckout(true)
  overrideIndexTriggers(false)
}

stages {
  stage ('Checkout'){
    steps {
      checkout scm
      script {
        env.commit_id = sh(script: "git rev-parse --short HEAD", returnStdout: true).trim()
      }
    }
  }

  stage('Docker Build') {
    steps {
      script {
        docker.build('anandsadhu/myapp')
        docker.withRegistry('https://hub.docker.com/','docker') {
          docker.image('anandsadhu/myapp').push(commit_id)
       }
      }
    }
  }


}
}
