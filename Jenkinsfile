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
        script {
            tag = sh(
                script: 'git describe --tags',
                returnStdout: true
            ).trim()
                env.TAG=tag
                sh 'echo ${TAG}'
        }
        sleep 5
      }
	}
	
	stage('Test load groovy function from outside') {
	    steps {
	        script {
	            def a = load "aaa.groovy"
                a.test()
            }
	    }
	}
	stage('Approval') {
	   steps {
        script {
            def a = load "aaa.groovy"
            a.approval()
        }
       }
    }
	
	
	/* build commit to UAT */
	stage('Deploy to uat') {
      steps {
        echo 'Building commit'
        script {
            tag = sh(
                script: 'git branch',
                returnStdout: true
            ).trim()
            
            env.TAG = tag
            println tag
            a.test()
        }
        
        sleep 3
      }
	}
  }
  
  post {
    always {
        echo 'Finished'
        /*deleteDir()  clean up our workspace */
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