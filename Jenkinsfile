@Library("lib2") _
import org.yaml.snakeyaml.Yaml

node {
	stage('Checkout') {
		checkout scm
	}
	stage('Parse Yaml'){
		echo 'Loading pipeline definition'
		Yaml parser = new Yaml()
		static Map configParser = parser.load(new File(pwd() + '/devops.yaml').text)
		print configParser
	}
	print configParser
	standardPipeline {
		projectName = configParser.name
		serverDomain = configParser.template
	}
}
