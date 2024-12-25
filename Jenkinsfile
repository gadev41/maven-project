pipeline
{
    agent {
  label 'DevServer'
}
tools {
  maven 'maven'
}

    stages 
    {
        stage('Build')
        {
            steps 
            {
                sh 'mvn clean package'
            }
            post 
            {
            success {
                archiveArtifacts artifacts: '**/target/*.war'
                    }
            }

        }
        
    }
}