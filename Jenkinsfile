@Library("lib2") _
import org.yaml.snakeyaml.Yaml

pipeline {
  agent any
  stages {
    stage('one') {
      steps {
        sh 'echo hotness > myfile.txt'
        script {
          // trim removes leading and trailing whitespace from the string
          myVar = readFile('myfile.txt').trim()
        }
        echo "${myVar}" // prints 'hotness'
      }
    }
    stage('Parse Yaml'){
      steps {
        echo 'Loading pipeline definition'
        script {
	  Yaml parser = new Yaml()
	  Map configParser = parser.load(new File(pwd() + '/devops.yaml').text)
	  a = configParser
        }
	echo "${a}"  
      }
    }
    stage('two') {
      steps {
        echo "${a}" // prints 'hotness'
      }
    }
    // this stage is skipped due to the when expression, so nothing is printed
    stage('three') {
      when {
        expression { myVar != 'hotness' }
      }
      steps {
        echo "three: ${a}"
      }
    }
  }
}
