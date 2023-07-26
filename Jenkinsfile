pipeline {


    agent { label 'JDK-17' } 

    triggers {
        pollSCM('* * * * *')

    
    }
    tools {
        jdk 'JDK_17'
         
    }

    stages { 
        stage('vcs') {
            steps {
                git url: 'https://github.com/Bhavana-2022/spc-project.git',
                    branch: 'devlop'
            }
                
        }
        stage('build and package') {
           steps {
               sh script: 'mvn package'
           }
        }  

        stage('results') {
            steps{
                archiveArtifacts artifacts: '**/target/*.jar'
                junit testResults: '**/target/surefire-reports/*.xml' 
            }
        }  
    
    }
    post { 
        success {
            mail subject: 'this project is success',
                 body: 'this spc project is success',
                 to: 'bhavanaamangarathi@gmail.com'


        }
        failure {
            mail subject: 'this project is failure',
                 body: 'this project has failed',
                 to: 'bhavanaamangarathi@gmail.com'
                
        }
    }


}

