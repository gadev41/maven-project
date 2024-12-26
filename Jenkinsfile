pipeline {
    agent {
        label 'DevServer'
    }
    parameters {
        choice choices: ['dev', 'prod'], name: 'select_environment'
        string(name: 'LASTNAME', defaultValue: 'DefaultLastName', description: 'Provide your last name')
    }

    environment {
        NAME = "gadev"
    }
    tools {
        maven 'maven'
    }
    stages {      
        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests=true'
                echo "hello ${env.NAME}  ${params.LASTNAME}"
            }
        }
        stage('Test') {
            parallel {
                stage('testA') {
                steps {
                    echo " This is test A"
                    sh "mvn test"
                }
                }
                stage('testB') {
                steps {
                    echo " This is test B"
                    sh "mvn test"
                }
                }
            }
        post {
            success {
                dir("webapp/target/") {
                    stash name: "maven-build", includes: "*.war"
                }
            }
        }
        }    
        stage('deploy_dev') {
            when { 
                expression {params.select_environment == 'dev'}
                beforeAgent true 
            }
            steps {
                script {
                    // Debugging: Check permissions before unstash
                    sh '''
                    echo "Checking permissions for /var/www/html/"
                    ls -ld /var/www/html || echo "Directory does not exist"
                    ls -l /var/www/html/webapp.war || echo "WAR file does not exist or cannot be accessed"
                    '''
                }
                dir("/var/www/html") {
                    unstash "maven-build"
                }
                sh """
                cd /var/www/html/
                if [ -f webapp.war ]; then
                    jar -xvf webapp.war
                else
                    echo "WAR file not found!"
                    exit 1
                fi
                """
            }
        }
    }
}


