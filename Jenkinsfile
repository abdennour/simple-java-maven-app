pipeline {
    agent {
      kubernetes {
        yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
    some-label: some-label-value
spec:
  containers:
  - name: maven
    image: maven:alpine
    command:
    - cat
    tty: true
  - name: busybox
    image: busybox
    command:
    - cat
    tty: true
"""
      }
    }
    stages {
        stage('Build') {
            steps {
                container('maven') {
                    sh 'mvn -B -DskipTests clean package'
                }
                
            }
        }
        stage('Test') {
            steps {
                container('maven') {
                    sh 'mvn test'
                }
                
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
                container('maven') {
                    sh './jenkins/scripts/deliver.sh'
                }
                
                container('busybox') {
                    sh 'echo Congratulations!! 🎉'
                }
                
            }
        }
    }
}
