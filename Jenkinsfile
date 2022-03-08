pipeline {
    agent {
        docker {
            image 'maven:3.8.1-adoptopenjdk-11' 
            args '-v /root/.m2:/root/.m2' 
        }
    }
    stages {
        stage('Maven Build') { 
            steps {
                sh 'mvn -B -DskipTests clean package' 
            }
        }
        stage('Image Build') { 
            steps {
                docker.build("springhelloworld:test.$BUILDNMBER")
            }
        }

    }
}
