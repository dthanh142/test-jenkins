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
    script {
	            def a = load "aaa.groovy"
    }
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
        timeout(60) {
            script {
                mail to: 'thanh.phamduc@vndirect.com.vn', 
                        from: 'jenkins-noreply@vndirect.com.vn',
                        subject: "Please approve ${jname} project to production", mimeType: 'text/html',
                        body: """Please <a href="${env.JOB_URL}${env.BUILD_ID}/input/">approve me</a>!"""
                input message: "Approve ${jname} to production"
                /*approvalMap = input id: 'test', message: 'Hello', ok: 'Proceed?', parameters: [choice(choices: 'uat\nstag\nprod', description: 'Select a environment for this build', name: 'ENV'), string(defaultValue: '', description: '', name: 'myparam')], submitter: 'thanh.phamduc,user2,group1', submitterParameter: 'APPROVER'*/
            }
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