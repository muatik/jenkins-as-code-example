pipeline {
    agent {
        docker {
            image 'maven:3-alpine' 
            args '-v /root/.m2:/root/.m2' 
        }
    }
    stages {
        stage('Prepare') {
            steps {
                sh '''export VERSION_QUALIFIER=1.0
                export NEW_VERSION=$VERSION_QUALIFIER.$(date +%Y%m%d%H%M%S)\\
                echo $NEW_VERSION'''
            }
        }
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
                    archiveArtifacts artifacts: 'build/libs/**/*.jar', fingerprint: true
                }
            }
        }
        stage('Deploy') {
            steps {
                sh 'echo \'uploading artifacts to some repositories\''
            }
        }
    }
}