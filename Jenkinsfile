pipeline {


    agent { label 'JDK-8' } 

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
               rtMavenDeployer (
                 id: 'spc-night-build',
                 releaseRepo: 'spring-libs-release',
                 snapshotRepo: 'spring-libs-snapshot',
                 serverId: 'spcnightbuild' 
               )
               rtMavenRun (
                  tool: 'DEFAULT',
                  pom: 'pom.xml',
                  goals: 'clean install',
                  deployerId:'spc-night-build'
               )
               rtPublishBuildInfo (
                 serverId: 'spcnightbuild' 

               )


           }
        }
        stage('SonarQube analysis') {
            steps {

                // performing sonarqube analysis with "withSonarQubeENV(<Name of Server configured in Jenkins>)"
                withSonarQubeEnv('SONAR_CLOUD') {
                // requires SonarQube Scanner for Maven 3.2+
                    sh 'mvn clean package sonar:sonar -Dsonar.organization=bhavanamangrathi123 -Dsonar.token=0c7e1fae7e6fe2f410f2c922665ef64f5336a71f -Dsonar.projectKey=bhavanamangrathi123_spring-pet-clinc'
                }
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

