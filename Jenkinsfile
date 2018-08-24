@Library("lib2") _
import org.yaml.snakeyaml.Yaml

node {
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
		echo "three: ${cp}"
	  	standardPipeline {
			projectName = ${cp.template}
			serverDomain = ${cp.approval}
	  	}
      	}
}
