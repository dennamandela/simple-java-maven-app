node {
    def mavenImage = 'maven:3.9.0'

    try {
        stage('Verify') {
            echo 'Verifying pom.xml file exists...'
            sh 'pwd'
            sh 'ls -la'
        }

        stage('Build') {
            docker.image(mavenImage).inside('-v //g/simple-java-maven-app:/app') {
                sh 'chmod -R 777 /app'  // Set permissions for the app directory
                sh 'ls -la /app'
                sh 'cd /app && mvn -B -Dmaven.repo.local=/app/.m2/repository -DskipTests clean package'
            }
        }

        stage('Test') {
            docker.image(mavenImage).inside('-v //g/simple-java-maven-app:/app') {
                sh 'cd /app && mvn test'
                junit '/app/target/surefire-reports/*.xml'
            }
        }
    } catch (Exception e) {
        currentBuild.result = 'FAILURE'
        throw e
    }
}