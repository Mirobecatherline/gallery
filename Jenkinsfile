
pipeline {
    agent any
    tools {
        nodejs  'NodeJS 22.9.0' 
    }

    environment {
      DEPLOY_HOOK_URL= "https://api.render.com/deploy/srv-crop0vi3esus73c58sg0?key=Lior9nQ5GZg"
    }
    
    stages {
        stage('Checkout Code') {
            steps {
              git credentialsId: 'mygithubconnection', url: 'https://github.com/Mirobecatherline/gallery.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                script {
                    sh 'npm install'
                }
            }
        }

        stage('Run Tests') {  
    steps {
        script {
            sh 'npm test'
        }
   }
}


        stage('Deploy to Render122') {
                        steps {
                            script {
                                def response = sh(script: """
                                    curl -X POST ${DEPLOY_HOOK_URL}
                                """, returnStdout: true).trim()
                                
                                echo "Deployment Response: ${response}"
                            }
                        }

                    }

        stage('Notify Slack') {
            steps {
               
                    slackSend(
                    botUser: true, 
                    channel: 'C07NPCS8DNY', 
                    color: '#0000FF',  
                    message: "Deployment successful! Build ID - ${env.BUILD_ID}. Check the deployed site: https://gallery-yogp.onrender.com", 
                    teamDomain: 'Student', 
                    tokenCredentialId: 'slackBot'
                )
                
            }
        }
    }
    post {
        failure {
              // mail to: 'catemirobe@gmail.com',
                //    subject: "Build Failed: ${currentBuild.fullDisplayName}",
                 //   body: "Something is wrong with ${env.JOB_NAME} ${env.BUILD_NUMBER}"
                 echo "pipeline failed"
        }

        success {
              // mail to: 'catemirobe@gmail.com',
                //    subject: "Build Failed: ${currentBuild.fullDisplayName}",
                 //   body: "Something is wrong with ${env.JOB_NAME} ${env.BUILD_NUMBER}"
                 echo "pipeline passsed"
        }
    }
}
