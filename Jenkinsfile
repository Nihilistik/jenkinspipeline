pipeline {
    agent any

    parameters {
        string(name:'tomcat_dev', defaultValue: 'relaxed_pike', description:'QA Server')
        string(name:'tomcat_pro', defaultValue: 'tomcatPRO', description:'PRO Server')
    }
    
    triggers {
        pollSCM('* * * * *')
    }
    stages {
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage ('Deployments'){
            parallel {
                stage ('Deploy to QA'){
                    steps{
                        sh "docker cp **/target/*.war ${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }
                stage ('Deploy to PRO'){
                    steps{
                        sh "docker cp **/target/*.war ${params.tomcat_pro}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}
