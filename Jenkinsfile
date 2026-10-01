pipeline {
    agent { label 'host1-arie' }
    environtment {
        SONAR_TOKEN = credentials('token-sonar')
        SONAR_HOST =  credentials('host-sonar') 
    }

    stages {
        stage('Pull SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/archiel92/simple-apps.git'
            }
        }
        
        stage('Build') {
            steps {
                sh'''
                cd apps
                npm install
                '''
            }
        }
        
        stage('Testing') {
            steps {
                sh'''
                cd apps
                npm test
                npm run test:coverage
                '''
            }
        }
        
        stage('Code Review') {
            steps {
                sh'''
                cd apps
                sonar-scanner \
                -Dsonar.projectKey=simple-apps \
                -Dsonar.sources=. \
                -Dsonar.host.url=${SONAR_HOST} \
                -Dsonar.token=${SONAR_TOKEN}
                '''
            }
        }

        stage('Deliver') {
            steps {
                input message: 'Are you sure?', ok: 'Yes'
            }
        }

        stage('Deploy') {
            steps {
                sh'''
                docker compose up --build -d
                '''
            }
        }
        
        
    }
}