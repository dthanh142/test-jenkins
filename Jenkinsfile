pipeline {
  agent any
  
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
	
  stages {
  
  
    /* build commit to UAT */
	stage('Deploy to uat') {
      steps {
        script {
            def a = load "/var/lib/jenkins/groovy_scripts/aaa.groovy"
            a.build_uat()
        }
      }
	}
  
    
    /* Sonar runner */
    stage('Sonar check') {
       steps {
         script {
            def a = load "/var/lib/jenkins/groovy_scripts/aaa.groovy"
            a.sonar()
         }
       }
    }
  
    
    /* Dockerfile */
    stage('Dockerfile') {
       steps {
         script {
            def a = load "/var/lib/jenkins/groovy_scripts/aaa.groovy"
            a.dockerfile()
         }
       }
    }
    
    
    /* build docker image */
    stage('build docker image') {
       steps {
         script {
            def a = load "/var/lib/jenkins/groovy_scripts/aaa.groovy"
            a.build_docker_image()
         }
       }
    }
    
    
    /* Send approval email*/
    stage('Approval') {
	   steps {
         script {
            def a = load "/var/lib/jenkins/groovy_scripts/aaa.groovy"
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
	        def a = load "/var/lib/jenkins/groovy_scripts/aaa.groovy"
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