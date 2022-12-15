pipeline {

    agent any

    stages {

        stage('Build') {

            steps {

                echo 'Running build automation'

                sh './gradlew build --no-daemon'

                archiveArtifacts artifacts: 'dist/trainSchedule.zip'

            }

        }

        stage( 'Build Docker Image') {

            when {

                branch 'master'

            }

            steps {

                script {

                    app = docker.build("freddyibe05/train-schedule")

                    app.inside {

                        sh 'echo $(curl localhoast:8080)'

                    }

                }

            }

        }

        stage('Push Docker Image') {

            when {

                branch 'master'

            }

            steps {

                script {

                    docker.withRegistry('http://registry.hub.docker.com', 'docker_hub_login') {

                        app.pudh("${env.BULD_NUMBER}")

                        app.push("latest")

                    }

                }

            }

        }

    }

}
