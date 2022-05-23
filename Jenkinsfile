pipeline{
    agent any
    environment {
        CI = 'true'
    }
    tools {nodejs "node"}
    stages {
        stage('check') {
            steps {
                sh 'npm config ls'
            }
        }
        stage('build') {
            steps {
                sh 'npm install'
                sh 'npm run build'
                archiveArtifacts artifacts: 'build/'
            }
        }
        
        stage('deliver'){
            
            environment {
                NETLIFY_AUTH_TOKEN = credentials('NETLIFY_AUTH_TOKEN')
                NETLIFY_SITE_ID = credentials('NETLIFY_SITE_ID')
                BUILD_URL = localhost:8080/snapshot/1
            }
            
            steps{
                sh 'npm install netlify-cli'
                // sh './node_modules/.bin/netlify deploy --site $NETLIFY_SITE_ID --auth $NETLIFY_ACCESS_TOKEN --prod --dir=build'
                sh 'npx netlify deploy --site $NETLIFY_SITE_ID --auth $NETLIFY_AUTH_TOKEN --dir build/ --prod'
            }
        }
    }
}