node {
    try {

        stage('Build') {
            sh 'mvn clean install'
        }
        stage('Test') {
            sh 'mvn test'
        }
        stage('Package') {
            sh 'mvn package'
        }
        currentBuild.result = 'SUCCESS'
    } catch (Exception e) {
        currentBuild.result = 'FAILURE'
        throw e
    } finally {
        // Post build actions
        if (currentBuild.result == 'SUCCESS') {
            echo 'CI Build and Test succeeded!'
        } else {
            echo 'CI Build or Test failed!'
        }
    }
}