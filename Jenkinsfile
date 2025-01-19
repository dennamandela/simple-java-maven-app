node {
    def mavenImage = 'maven:3.9.0'

    try {
        stage('Verify') {
            echo 'Verifying pom.xml file exists...'
            sh 'pwd'
            sh 'ls -la'
        }

        stage('Build') {
            docker.image(mavenImage).inside('-v //g/simple-java-maven-app:/root/app') {
                sh 'cd /root/app && mvn -B -DskipTests clean package'
            }
        }

        stage('Test') {
            docker.image(mavenImage).inside('-v //g/simple-java-maven-app:/root/app') {
                sh 'cd /root/app && mvn test'
                junit '/root/app/target/surefire-reports/*.xml'
            }
        }
    } catch (Exception e) {
        currentBuild.result = 'FAILURE'
        throw e
    }
}