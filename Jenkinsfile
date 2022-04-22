pipeline {
agent any
triggers {
  pollSCM('H/2 * * * *')
}
stages {
  stage ('Checkout'){
    steps {
      checkout scm
    }
  }
  stage('Docker Build') {
    steps {
			sh '''
			echo hostname
			'''
    }
  }
}
}
