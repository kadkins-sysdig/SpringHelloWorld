pipeline {
  agent any
  stages {
    stage('Checkout Scm') {
      steps {
        git branch: 'main', credentialsId: 'Kadkins-Sysdig-Jenkins-Test1', url: 'https://github.com/kadkins-sysdig/SpringHelloWorld.git'
      }
    }

    stage('Add Mutator File') {
      steps {
        sh '''echo $RANDOM | md5sum > $WORKSPACE/src/main/resources/mutator'''
        sh '''cat $WORKSPACE/src/main/resources/mutator'''
      }
    }

    stage('Maven Build') {
      steps {
        sh 'mvn package'
      }
    }

    stage('Docker Build') {
      steps {
        script {
            docker.build("test-freestyle:1.${BUILD_NUMBER}")
        }
      }
    }

    stage('Generate Sysdig Secure Images File') {
      steps {
        sh 'echo "test-freestyle:1.${BUILD_NUMBER} Dockerfile" > $WORKSPACE/sysdig_secure_images'
      }
    }

    stage('Sysdig Secure Image Scan') {
        environment {
            http_proxy = "http://10.0.1.83:8887/"
            https_proxy = "http://10.0.1.83:8887/"
            no_proxy = "127.0.0.1,localhost,10.0.1.0/24,10.0.10.0/24"
            DOCKER_HOST = "unix:///var/run/docker.sock"
        }
        steps {
            script{                    
                env.http_proxy
                env.https_proxy
                env.no_proxy
                env.DOCKER_HOST
                sh 'printenv | sort'
                echo "SysDig Scan"
                /* OLD ENGINE
                sysdig bailOnFail: false, bailOnPluginFail: false, engineCredentialsId: 'sysdig-secure-api-credentials', engineurl: 'https://secure.sysdig.com', forceScan: false, name: 'sysdig_secure_images', inlineScanning: true
                */
                /* NEW ENGINE */
                sysdigImageScan  bailOnFail: false, bailOnPluginFail: false, engineCredentialsId: 'sysdig-secure-api-credentials', engineurl: 'https://secure.sysdig.com', forceScan: false, imageName: test-freestyle:1.${BUILD_NUMBER}, inlineScanning: true
              /* */
            }
        }        
    }
  }
}
