pipeline{
    agent any
    environment{
        SONAR_HOME= tool "Sonar"
    }
    stages{
        stage("Clone Code from GitHub"){
            steps{
               git url: "https://github.com/ParthParmar2711/wanderlusts", branch: "master"
            }
        }
        stage("SonarQube Quality Analysis"){
            steps{
                  withSonarQubeEnv("Sonar"){
                      sh "$SONAR_HOME/bin/sonar-scanner -Dsonar.projectName=wanderlust -Dsonar.projectKey=wanderlust"
                  }
            }
        }
        stage("OWASP Dependency Check"){
            steps{
               dependencyCheck additionalArguments: '--scan ./', odcInstallation: 'dc'
               dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
        stage("Sonar Quality Gate Scan"){
            steps{
                timeout(time: 2, unit: "MINUTES"){
                    waitForQualityGate abortPipeline: false
                }
            }
        }
        stage("Trivy File System Scan"){
            steps{
               sh "trivy fs --format table -o trivy-fs-report.html ."  
            }
        }
        stage("Deploy using Docker Compose"){
            steps{
                sh "docker-compose down"
                sh "docker-compose up -d"
            }
        }
        stage("Restart Nginx") {
            steps {
                script {
                   sleep(time: 10, unit: 'SECONDS')
                   sh "docker exec nginx nginx -s reload"  
               } 
            }
        }
    }
}
