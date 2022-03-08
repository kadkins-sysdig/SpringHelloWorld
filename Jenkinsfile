pipeline {
    stages {
        stage('Build') { 
            agent {
                docker {
                    image 'maven:3.8.1-adoptopenjdk-11' 
                    args '-v /root/.m2:/root/.m2' 
                }
            }
            steps {
                sh 'mvn -B -DskipTests clean package' 
            }
        }
        stage('Hello') { 
            steps {
                echo "Hello!"
            }
        }
    }
}
