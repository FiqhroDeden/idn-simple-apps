pipeline {
    agent { label 'docker-label' }
    
    tools {nodejs "nodejs-simpleapps"}

    stages {
        stage('Checkout SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/FiqhroDeden/idn-simple-apps.git'
            }
        }
        stage('Build') {
            steps {
                sh '''cd /home/hari-1/simple-app
                npm install'''
            }
        }
        stage('Testing') {
            steps {
                sh '''
                cd /home/hari-1/simple-app
                npm test
                npm run test:coverage
                '''
            }
        }
        stage('Code Review') {
            steps {
                sh '''
                cd /home/hari-1/simple-app
                sonar-scanner \
                -Dsonar.projectKey=simple-app \
                -Dsonar.sources=. \
                -Dsonar.host.url=https://172.23.14.4:9000 \
                -Dsonar.login=sqp_c2be27f9008dc49c37ad4eb6d174c3620340a9d3
                '''
            }
        }
        stage('Deploy compose') {
            steps {
                sh '''
                cd /home/hari-1/simple-app
                docker compose build
                docker compose up -d
                '''
            }
        }
        stage('Build and Push Image') {
            steps {
                sh '''
                cd /home/hari-1/simple-app 
                docker build -t dedhens/simple-app . 
                docker push dedhens/simple-app
                '''
            }
        }
    }
}