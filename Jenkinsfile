pipeline {
  agent any
  environment {
    jname = 'test'
	port = '9000'
  }

  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
	
  stages {
    /* build tag to prod*/
    stage('Deploy to production') {
		when {
			buildingTag()
		}
      steps {
        echo 'Building tag'
        def tag = sh(script: 'git describe --tags $(git rev-list --tags --max-count=1)', returnStdout: true).trim()
        sleep 5
      }
	}
	
	
	/* build commit to UAT */
	stage('Deploy to uat') {
      steps {
        echo 'Building commit'
        sleep 5
      }
	}
  }
  
  post {
    always {
        echo 'Finished'
        deleteDir() /* clean up our workspace */
    }
    success {
        echo 'Succeeded!'
    }
    unstable {
        echo 'Unstable :/'
    }
    failure {
        echo 'Failed :('
    }
  }
}
