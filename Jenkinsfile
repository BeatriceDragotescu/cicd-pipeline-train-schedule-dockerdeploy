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
        stage('Build Docker Image') {
            when {
                branch 'master'
            }
            stage {
                app = docker.build("BeatriceDragotescu/train-schedule")
                app_inside {
                    sh 'echo $(curl localhost:8080)'
                }
            }
        }
    }
    satge('Push Docker Image') {
        when {
            steps {
                script {
                    dockerwithRegistry('https://registry.hwb.docker.com', 'docker_hub_login') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }
    }
}
