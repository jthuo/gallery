pipeline {
    agent any
    tools {nodejs "node"}
    stages {
        stage('Start') {
            steps {
                echo 'The build has started'
            }
        }
        stage('Clone the repository') {
            steps {
                git url: 'https://github.com/jthuo/gallery.git', branch: 'master'
            }
        }
        stage('Get Latest Commit') {
            steps {
                sh '''
                   export COMMIT=$(git log --oneline | awk '{print $1}' | head -n 1)
                   echo $COMMIT
                   '''
            }
        }
        stage('Install dependencies') {
            steps {
                sh '''
                   npm install
                   '''
            }
        }
        stage('Run Tests') {
            steps {
                sh '''
                npm test
                '''
            }
        }
        stage('Deploy to Render') {
            steps {
                sh '''
                   curl -X POST https://api.render.com/deploy/srv-cgc4ib82qv267ubalgi0?key=$COMMIT
                   '''
            }
        }
        stage('End') {
            steps {
                echo 'The build has ended'
            }
        }
    }
    post {
            failure {
                mail to: 'jthuo121@gmail.com',
                subject:"FAILURE: ${currentBuild.fullDisplayName}",
                body: "Test Complete Build failed."
            }
        }
}