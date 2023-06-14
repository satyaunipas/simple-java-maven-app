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
                input message: 'Lanjutkan ke tahap Deploy?', parameters: [
                    [$class: 'BooleanParameterDefinition', defaultValue: false, description: 'Proceed atau Abort', name: 'Proceed']
                ]
            }
        }
        stage('Deploy') {
            when {
                expression { params.Proceed == true }
            }
            steps {
                sh './jenkins/scripts/deliver.sh'

                // Pause for 1 minute
                sleep time: 1, unit: 'MINUTES'
            }
        }
    }
}
