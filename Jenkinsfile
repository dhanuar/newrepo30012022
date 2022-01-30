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
              withSonarQubeEnv(credentialsId: 'sonar7') {
                 sh "mvn sonar:sonar"
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
