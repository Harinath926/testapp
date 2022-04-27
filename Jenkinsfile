pipeline {
  agent {
     kubernetes {
      defaultContainer 'jenkins-slave'
      yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: shell
    image: ubuntu
    command:
    - sleep
    args:
    - infinity
"""
    }
}

triggers {
  pollSCM('H/2 * * * *')
}

stages {
  stage('Docker Build') {
    steps {
			sh '''
			echo hostname
			'''
    }
  }
}
}
