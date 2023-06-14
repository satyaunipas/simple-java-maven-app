pipeline {
    agent {
        docker {
            image 'maven:3.9.0'
            args '-v /root/.m2:/root/.m2'
        }
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
        stage('Manual Approval') {
            steps {
                script {
                    def userInput = input message: 'Lanjutkan ke tahap Deploy?', parameters: [choice(name: 'action', choices: ['Proceed', 'Abort'], description: 'Pilih Proceed untuk melanjutkan atau Abort untuk menghentikan pipeline')]
                    if (userInput == 'Abort') {
                        error('Pipeline aborted')
                    }
                }
            }
        }
        stage('Deploy') 
            steps {
                sh './jenkins/scripts/deliver.sh'

                // Pause for 1 minute
                sleep time: 1, unit: 'MINUTES'
            }
        }
    }
}
