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

     stage('Install Node.js and npm') {
            steps {
                script {
                    // Check if Node.js is installed
                    def nodeInstalled = sh(script: 'node -v', returnStatus: true) == 0
                    if (!nodeInstalled) {
                        // Install Node.js and npm (for Ubuntu/Debian)
                        sh '''
                        sudo apt update
                        sudo apt install -y nodejs npm
                        '''
                    }
                }
            }
        }
        stage('Build') {
            steps {
                sh 'npm install'  // Install npm packages
                sh 'npm run build'  // Example build command
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
