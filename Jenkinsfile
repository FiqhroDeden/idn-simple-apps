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
                sh '''
                cd apps
                npm install'''
            }
        }
        stage('Testing') {
            steps {
                sh '''
                cd apps
                npm test
                npm run test:coverage
                '''
            }
        }
        stage('Code Review') {
            steps {
                sh '''
                cd apps
                sonar-scanner   -Dsonar.projectKey=simple-app   -Dsonar.sources=.   -Dsonar.host.url=http://172.23.14.4:9000   -Dsonar.login=sqp_c2be27f9008dc49c37ad4eb6d174c3620340a9d3
                '''
            }
        }
        stage('Deploy compose') {
            steps {
                sh '''
                docker compose build
                docker compose up -d
                '''
            }
        }
        stage('Build and Push Image') {
            steps {
                sh '''
                cd apps
                docker build -t dedhens/simple-app . 
                docker push dedhens/simple-app
                '''
            }
        }
    }
}