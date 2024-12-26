pipeline {
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
                expression {params.select_environment == 'prod'}
                beforeAgent true 
            }
            steps {
                dir("/var/www/html") {
                    unstash "maven-build"
                }
                sh """
                cd /var/www/html/
                jar -xvf webapp.war
                """
            }
        }
        stage('deploy_prod') {
            when { 
                expression {params.select_environment == 'prod'}
                beforeAgent true 
            }
            agent {label 'ProdServer'}
            steps {
                timeout(time:5, unit:'DAYS') {
                    input message: 'Deployment approved?'
                }
                dir("/var/www/html") {
                    unstash "maven-build"
                }
                sh """
                cd /var/www/html/
                jar -xvf webapp.war
                """
            }
        }
    }
}

