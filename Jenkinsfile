pipeline {
    agent any
    
    //environment{
        //SCANNER_HOME= tool 'sonar-scanner'
    //}
    
    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'test', url: 'https://github.com/Noah-linux/DevOps-CI-Project.git'
            }
        }
        
        //stage('OWASP FS SCAN') {
            //steps {
                //dependencyCheck additionalArguments: '--scan ./app/backend --disableYarnAudit --disableNodeAudit', odcInstallation: 'DC'
                    //dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
          //  }
       // }
        
        stage('TRIVY FS SCAN') {
            steps {
                sh "trivy fs ."
            }
        }
        
        //stage('SONARQUBE ANALYSIS') {
           // steps {
                //withSonarQubeEnv('sonar') {
                   // sh " $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Bank -Dsonar.projectKey=Bank "
              //  }
           // }
      //  }
        
        
        stage('Install Dependencies') {
            steps {
                sh "npm install"
           }
        }


        
        stage('Backend') {
            steps {
                dir('/root/.jenkins/workspace/Bank/app/backend') {
                    sh "npm install"
                }
            }
        }
        
        stage('frontend') {
            steps {
                dir('/root/.jenkins/workspace/Bank/app/frontend') {
                    sh "npm install"
                }
            }
        }
        
        stage('Deploy to Conatiner') {
            steps {
                sh "npm run compose:up -d"
            }
        }
    }
}
