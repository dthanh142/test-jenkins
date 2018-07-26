node {
    a = load "/var/lib/jenkins/groovy_scripts/aaa.groovy"
}

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
            a.build_uat()
        }
      }
	}
  
    
    /* Sonar runner */
    stage('Sonar check') {
       steps {
         script {
            a.sonar()
         }
       }
    }
  
    
    /* Dockerfile */
    stage('Dockerfile') {
       steps {
         script {
            a.dockerfile()
         }
       }
    }
    
    
    /* Send approval email*/
    stage('Approval') {
	   steps {
         script {
            a.approval()
         }
       }
    }
    
    
    /* build docker image */
    stage('build docker image') {
       steps {
         script {
            a.build_docker_image()
         }
       }
    }
    
    
    /* Send approval email*/
    stage('Approval') {
	   steps {
         script {
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