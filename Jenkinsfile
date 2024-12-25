pipeline
{
agent {
  label 'DevServer'
}
parameters {
  string defaultValue: 'boopathy', name: 'LASTNAME'
}

environment {
    NAME = "gadev"
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
                echo "hello $NAME  ${params.LASTNAME}"
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