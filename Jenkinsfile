pipeline {
    agent {
        docker {
            image 'maven:3.9.2'
            args '-v $PWD/.m2:/root/.m2' // Mount Maven's repository inside the current workspace
        }
    }
    environment {
        MAVEN_OPTS = '-Dmaven.repo.local=$PWD/.m2/repository'  // Point to the local repository inside the workspace
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
    }
}
