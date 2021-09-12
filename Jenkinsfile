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
        stage('Build Docker image ') {
            when {
                 branch 'master'
                
            }
            steps {
                script {
                          app = dcoker.build("mobaqii/train-schedule")
                    app.inside {
                                    sh 'echo $(curl localhost:8080)'
                    }
                }
                
            } 
            
        }
        stage (' push docer image ') { 
            when {
                branch 'master '
            }
         steps { 
            script {
                docker.Withregistry('https://registry.hub.docker.com','docker_hub_login') {
                     app.push ("${env.BUILD_NUMBER}")
                     app.push ("latest")
                  }
               }
            }
            
         }
     }
   }

