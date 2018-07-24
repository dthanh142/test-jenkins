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
	
	stage('Approval') {
	   steps {
        timeout(60) {
            script {
                mail to: 'thanh.phamduc@vndirect.com.vn', 
                        subject: 'Ready to roll?', mimeType: 'text/html',
                        body: """Please <a href="${env.JOB_URL}${env.BUILD_ID}/input/">approve me</a>!"""
                input message: 'Ready?'
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