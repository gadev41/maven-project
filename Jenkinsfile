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


        }
        stage('test')
        {
            parallel {
                stage('testA')
                {
                    steps{
                        echo " This is test A"
                    }
                    
                }
                stage('testB')
                {
                    steps{
                        echo " This is test B"
                    }
                }
            }
        }
            post {
            success {
                    archiveArtifacts artifacts: '**/target/*.war'
                        }
                }
    }
}