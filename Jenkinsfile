pipeline {
    agent {
        docker {
            image 'maven:3.9.2'
            args '-v $PWD/.m2:/root/.m2' // Mount Maven repository inside workspace
        }
    }
    environment {
        MAVEN_OPTS = '-Dmaven.repo.local=$PWD/.m2/repository' // Point to the local repository
    }
    stages {
        stage('Build') {
            steps {
                echo '🚀 Memulai Build...'
                sh 'mvn -B -DskipTests clean package'
            }
        }

        stage('Test') {
            steps {
                echo '✅ Menjalankan Test...'
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage('Manual Approval') {
            steps {
                script {
                    try {
                        input message: 'Lanjutkan ke tahap Deploy? (Klik "Proceed" untuk melanjutkan atau "Abort" untuk membatalkan)'
                    } catch (err) {
                        error("🚨 Deployment dibatalkan oleh user.")
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                echo '🚀 Deploying aplikasi...'
                sh './jenkins/scripts/deploy.sh'

                echo '✅ Aplikasi berhasil di-deploy. Menunggu 1 menit sebelum otomatis dihentikan...'
                sh 'sleep 60' // Menjeda pipeline selama 1 menit

                echo '🔴 Menghentikan aplikasi setelah 1 menit...'
                sh './jenkins/scripts/stop.sh'
                echo '✅ Aplikasi berhasil dihentikan.'
            }
        }
    }
}
