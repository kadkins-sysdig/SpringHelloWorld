pipeline {
    agent {
        docker {
            image 'maven:3.8.1-adoptopenjdk-11' 
            args '-v /root/.m2:/root/.m2' 
        }
    }
    stages {
        stage('Build') { 
            steps {
                sh 'mvn -B -DskipTests clean package' 
            }
        }
        stage('Hello') { 
            steps {
                echo "Hello!"
            }
        }
        stage('Build Container') { 
            steps {
                script {
                    def customImage = docker.build("my-image:${env.BUILD_ID}")
                    customImage.inside {
                        sh 'make test'
                    }                
                }
            }
        }
    }
}
