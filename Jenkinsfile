@Library("lib3") _
    
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
