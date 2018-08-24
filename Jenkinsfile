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
        Yaml parser = new Yaml()
        static Map configParser = parser.load(new File(pwd() + '/devops.yaml').text)
        print configParser
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
