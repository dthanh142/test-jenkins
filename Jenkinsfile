@Library("lib2") _
import org.yaml.snakeyaml.Yaml

pipeline {
  agent any
  stages {
    
    stage('Parse Yaml'){
      steps {
        echo 'Loading pipeline definition'
        script {
	  Yaml parser = new Yaml()
	  Map configParser = parser.load(new File(pwd() + '/devops.yaml').text)
	  cp = configParser
        }
	echo "${cp}"
      }
    }
    stage('two') {
      steps {
        echo "${cp}" // prints 'hotness'
      }
    }
    // this stage is skipped due to the when expression, so nothing is printed
    stage('three') {
      when {
        expression { cp.template != 'java' }
      }
      steps {
        echo "three: ${cp.template}"
	script {
	  standardPipeline {
		projectName = cp.name
		serverDomain = cp.template
	  }
	}
      }
    }
  }
}
