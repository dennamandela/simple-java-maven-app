node {
    def mavenImage = 'maven:3.9.0'

    try {
        stage('Verify') {
            echo 'Verifying pom.xml file exists...'
            sh 'pwd'
            sh 'ls -la'
        }

        stage('Build') {
            docker.image(mavenImage).inside('-v /root/.m2:/root/.m2 -v $(pwd):/workspace') {
                dir('simple-java-maven-app') {
                    sh 'mvn -B -DskipTests clean package'
                }
            }
        }

        stage('Test') {
            docker.image(mavenImage).inside('-v /root/.m2:/root/.m2 -v $(pwd):/workspace') {
                dir('simple-java-maven-app') {
                    sh 'mvn test'
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
    } catch (Exception e) {
        currentBuild.result = 'FAILURE'
        throw e
    }
}