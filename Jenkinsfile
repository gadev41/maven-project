pipeline
{
agent {
  label 'DevServer'
}
parameters {
  choice choices: ['dev', 'prod'], name: 'select_environment'
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
                sh 'mvn clean package -DskipTests=true'
                echo "hello $NAME  ${params.LASTNAME}"
            }


        }
    stage('Test')
    {
        parallel
        {
            stage('testA')
            {
              agent {label 'DevServer'}
              steps
              {
              echo " This is test A"
              sh "mvn test"
              }
            }
            stage('testB')
            {
              agent {label 'DevServer'}
              steps
              {
              echo " This is test B"
              sh "mvn test"
              }
            }
        }
      post 
      {
      success {
        dir("webapp/target/")
        {
        stash name: "maven-b", includes: "*.war"
        }
           }
      }
    }    
    stage('deploy_dev')
        {
            when { expression {params.select_environment == 'dev'}
            beforeAgent true }
            agent { label 'DevServer' }
            steps
            {
                dir("/var/www/html")
                {
                    unstash "maven-b"
                }
                sh """
                cd /var/www/html/
                jar -xzf webapp.war
                """
            }
        }
    }
}


