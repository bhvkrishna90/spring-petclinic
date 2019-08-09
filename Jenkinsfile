pipeline{
    agent any
    stages{
        stage("SendNotifications"){
            steps{
                echo ${env.JOB_STATUS}
                office365ConnectorSend color: 'Green', message: '${env.JOB_STATUS} ${env.JOB_NAME} ${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)', status: '${env.JOB_STATUS}', webhookUrl: '${env.TeamsWebhook}'
            }
        }
        stage("SCM Checkout"){
            steps{
                figlet "Checkout"
                echo "========Cloning GitHUb repository========"
            }
            post{
                always{
                    echo "========always========"
                }
                success{
                    echo "========Source Code Clonned successfully ========"
                }
                failure{
                    echo "========Source code failed to clone ========"
                }
            }
        }
        stage("sonarqubeAnalysis"){
            steps{
                figlet "Static code Analysis"
                echo "========Analysing SonarScannerReport========"
                def scannerHome = tool 'SSonarqube'
                withSonarQubeEnv('SonarQube') {
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }
        stage("Quality Gate"){
            steps{
                timeout(time: 1, unit: 'HOURS') 
                def qg = waitForQualityGate() 
                if (qg.status != 'OK') {
                    error "Pipeline aborted due to quality gate failure: ${qg.status}"
                }
            }
        }
        stage("SendCompletedNotification"){
            steps{
                office365ConnectorSend color: 'Green', message: '${env.JOB_STATUS} ${env.JOB_NAME} ${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)', status: '${env.JOB_STATUS}', webhookUrl: '${env.TeamsWebhook}'
            }
        }
    }
}
