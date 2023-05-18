pipeline {
    agent any
    tools{
        nodejs "node"
    }
    stages{
        stage ('Clone Repository'){
            steps{
                git "https://github.com/Mbuthiapeter/gallery.git"
            }
        }
	stage('Installing Dependencies'){
		steps{
		sh 'npm install express nodemon  ejs multer'
		}
	}
        stage ('Build project'){
            steps{
                sh 'npm run build'
            }
        }
        stage ('Tests'){
            steps{
                sh 'npm run test'
            }
        }
        stage('Deploy to Heroku') {
            steps {
                withCredentials([usernameColonPassword(credentialsId: 'heroku', variable: 'HEROKU_CREDENTIALS' )]){
                sh 'git push https://${HEROKU_CREDENTIALS}@git.heroku.com/stormy-fortress-37307.git master'
            }
            }
        }
        
    }
    post {
        success {
            emailext attachLog: true, 
                body:
                    """
                    <p>EXECUTED: Job <b>\'${env.JOB_NAME}:${env.BUILD_NUMBER})\'</b></p>
                    <p>
                    View console output at 
                    "<a href="${env.BUILD_URL}">${env.JOB_NAME}:${env.BUILD_NUMBER}</a>"
                    </p> 
                      <p><i>(Build log is attached.)</i></p>
                    """,
                subject: "Status: 'SUCCESS' -Job \'${env.JOB_NAME}:${env.BUILD_NUMBER}\'", 
                to: 'mbuthiapetermbuthia@gmail.com'
	script {
                slackSend(channel: '#week-6-ip', message: "Build successful! :white_check_mark:")
            }
        }
        failure {
            emailext attachLog: true, 
                body:
                    """
                    <p>EXECUTED: Job <b>\'${env.JOB_NAME}:${env.BUILD_NUMBER})\'</b></p>
                    <p>
                    View console output at 
                    "<a href="${env.BUILD_URL}">${env.JOB_NAME}:${env.BUILD_NUMBER}</a>"
                    </p> 
                      <p><i>(Build log is attached.)</i></p>
                    """,
                subject: "Status: FAILURE -Job \'${env.JOB_NAME}:${env.BUILD_NUMBER}\'", 
                to: 'mbuthiapetermbuthia@gmail.com'

	script {
                slackSend(channel: '#week-6-ip', message: "Build failed! :x:")
            }
        }
}
    
    
}
