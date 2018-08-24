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
	  a = configParser
        }
	echo "${configParser}"  
      }
    }
    stage('two') {
      steps {
        echo "${configParser}" // prints 'hotness'
      }
    }
    // this stage is skipped due to the when expression, so nothing is printed
    stage('three') {
      when {
        expression { myVar != 'hotness' }
      }
      steps {
        echo "three: ${configParser}"
      }
    }
  }
}
