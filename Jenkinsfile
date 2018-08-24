@Library("lib3") _
  
pipeline {
  agent any
    stages {
        stage('Install') {
          steps {
            script {
              faas.install {
                install_path = '/tmp'
                platform = 'linux'
                version = '0.5.1'
              }
            }
          }
        }
    }
}
