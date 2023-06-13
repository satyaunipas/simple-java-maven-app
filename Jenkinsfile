node {
    docker.image('maven:3.9.0').inside('-v /root/.m2:/root/.m2') {
        stage('Build') {
            sh 'javac -version'
            sh 'mvn -B -DskipTests clean package'
        }
        stage('Test') {
            sh 'java -version'
            sh 'mvn test'
            junit 'target/surefire-reports/*.xml'
        }
    }
}


