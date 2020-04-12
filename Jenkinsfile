pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
            stage('Docker image build') {
                when {
                    branch 'master'
                }
                script {
                    app = docker.build("nimishmehta8779/train-schedule")
                    app.inside {
                        sh 'echo $(curl localhost:8080)'
                    }
                }
                stage ('Push to Docker hub'){
                    when {
                        branch 'master'
                    }
                    script {
                        docker.withregistry('http://registry.hub.docker.com', 'docker.hub.login')
                        app.push ("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }
    }
}
