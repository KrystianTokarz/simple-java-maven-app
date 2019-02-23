def username = 'Jenkins'

pipeline {
    agent any

     parameters {
            string(name: 'Greeting', defaultValue: 'Hello', description: 'How should I greet the world?')
        }

    stages {
       stage('Example') {
                steps {
                    echo "${params.Greeting} World!"
                    echo "I said, Hello Mr. ${username} on ${env.JENKINS_URL}"
                }
            }

        stage('Build') {
            steps {
                bat 'mvn -B -DskipTests clean package'
                archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
            }
        }
         stage('Test') {
            steps {
                bat 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
         stage('Deliver') {
                    when {
                       expression {
                         currentBuild.result == null || currentBuild.result == 'UNSTABLE'
                       }
                     }
                    steps {

                        bat './jenkins/scripts/deliver.bat'
                     }
                }

    }
}
