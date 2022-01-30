pipeline{
    agent any
    triggers {
      pollSCM '* * * * *'
    }
    stages{
        stage("Maven Build"){
            when {
               expression{
                   env.BRANCH_NAME.equals("develop") ||
                   env.BRANCH_NAME.startsWith("feature")
               }
            }
            steps{
               sh "mvn package"
            }
        }
        stage("sonar analysis"){
            when {
                expression{
                   env.BRANCH_NAME.equals("develop") ||
                   env.BRANCH_NAME.startsWith("feature")
               }
            }
            steps{
              withSonarQubeEnv('sonar7') {
                 sh "mvn sonar:sonar"       
        }
            }
        }
        stage("SonarQube Status"){
            when {
                branch "develop"
            }
            steps{
                script{
               timeout(time: 1, unit: 'HOURS') {
                //    For this to work, we should add webhook in sonar
                //    http://172.31.3.50:8080/sonarqube-webhook/
                    def qg = waitForqualitygate()
                    if (qg.status != 'OK') {
                        error "Pipeline aborted due to quality gate failure: ${qg.status}"
                    }
                }
               }
            }
        }
        stage("nexus artifact"){
            when {
                expression{
                   env.BRANCH_NAME.equals("develop") ||
                   env.BRANCH_NAME.startsWith("feature")
               }
            }
            steps{
               echo "nexus artifact upload"
            }
        }
        stage("Deploy to QA"){
            when {
                branch "qa"
            }
            steps{
               echo "nexus artifact upload"
            }
        }
        stage("Deploy to production"){
            when {
                branch "master"
            }
            steps{
               echo "deploy to master"
            }
        }
    }
}
