@Library("lib2") _

node {
	properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '5')), gitLabConnection('gitlab')])
  stage("Checkout code") {
    checkout scm
  }
  
  stdPipeline()
}
