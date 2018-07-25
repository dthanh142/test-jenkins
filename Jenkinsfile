pipeline {
  agent any
  environment {
	port = '9000'
  }
  
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
	
  stages {
  
  
    /* build commit to UAT */
	stage('Deploy to uat') {
      steps {
        script {
            def a = load "aaa.groovy"
            a.build_uat()
        }
      }
	}
  
  
    /* Send approval email*/
    stage('Approval') {
	   steps {
         script {
            def a = load "aaa.groovy"
            a.approval()
         }
       }
    }
    
    
    /* Build tag to prod*/
    stage('Deploy to production') {
      when {
    	buildingTag()
      }
      steps {
        script {
	        def a = load "aaa.groovy"
            a.build_tag_to_prod()
        }
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