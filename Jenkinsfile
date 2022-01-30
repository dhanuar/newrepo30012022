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
                branch "develop"
            }
            steps{
              echo "sonar qube analysis"
            }
        }
        stage("nexus artifact"){
            when {
                branch "develop"
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
