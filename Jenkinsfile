pipeline{
    agent any
    stages{
        stage("SCM Checkout"){
            steps{
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
        stage(sonarqubeAnalysis){
            steps{
                echo "========Analysing SonarScannerReport========"
            }
        }
    }
}
 
