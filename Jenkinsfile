@Library("lib2") _
import org.yaml.snakeyaml.Yaml

node {
  agent any
	stage('Checkout') {
		checkout scm
	}
    	stage('Parse Yaml'){
      		echo 'Loading pipeline definition'
		Yaml parser = new Yaml()
		Map configParser = parser.load(new File(pwd() + '/devops.yaml').text)
		cp = configParser
		echo "${cp}"
    	}
    
    // this stage is skipped due to the when expression, so nothing is printed
    	stage('three') {
     		when {
			expression { cp.template != 'java' }
      		}
		echo "three: ${cp.template}"
	  	standardPipeline {
			projectName = cp.name
			serverDomain = cp.template
	  	}
      	}
}
