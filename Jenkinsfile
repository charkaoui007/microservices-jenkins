pipeline {
    agent any
    stages {
        stage("verify tooling") {
            steps {
                bat '''
                    docker version
                    docker info
                    docker-compose version
                    curl --version
                '''
            }
        }
        stage('Prune Docker data') {
            steps {
                bat 'docker system prune -a --volumes -f'
            }
        }
        stage('Build Docker images') {
            steps {
                bat 'docker-compose build --no-cache --pull --parallel'
            }
        }
        stage('Start containers') {
            steps {
                bat 'docker-compose up -d --no-color --wait'
                bat 'docker-compose ps'
            }
        }
    }
    post {
        always {
            bat 'docker-compose down --remove-orphans -v'
            bat 'docker-compose ps'
        }
    }
}
