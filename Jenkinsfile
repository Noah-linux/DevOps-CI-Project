pipeline {
    agent any

    tools {
        nodejs 'node22' // 'nodejs' should match the name you configured in Global Tool Configuration
    }

    parameters {  
        string(name: 'AppPort', defaultValue: '3000', description: 'Port to run the application')
        string(name: 'JFrogURL', defaultValue: 'ncloud.jfrog.io', description: 'JFrog Artifactory URL')
        string(name: 'RepositoryName', defaultValue: 'bank', description: 'Repository name for Docker image') // Parameterize repository name
        string(name: 'ImageName', defaultValue: 'bank', description: 'Docker image name') // Parameterize image name
    }

    environment {
        DOCKER_IMAGE_NAME = "${params.JFrogURL}/${params.RepositoryName}/${params.ImageName}:${env.BUILD_NUMBER}" // Parameterized repository and image names
    }

    stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: "https://github.com/Noah-linux/DevOps-CI-Project.git"
            }
        }

        stage('TRIVY FS SCAN') {
            steps {
                sh "trivy fs ."
            }
        }

        stage('Backend') {
            steps {
                sh '''
                    mkdir -p $WORKSPACE/app/backend
                    cd $WORKSPACE/app/backend
                    npm install
                '''
            }
        }

        stage('Frontend') {
            steps {
                sh '''
                    mkdir -p $WORKSPACE/app/frontend
                    cd $WORKSPACE/app/frontend
                    npm install
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    sh '''
                        cd $WORKSPACE/app
                        docker compose build
                    '''
                }
            }
        }

        stage('Deploy to Container') {
            steps {
                script {
                    // Deploy the application using docker-compose
                    sh '''
                        cd $WORKSPACE/app
                        docker compose up -d
                    '''
                }
            }
        }
    }

    post {
        always {
             //Clean up Docker containers after the pipeline finishes
           script {
               sh '''
                    cd $WORKSPACE/app
                    docker-compose down
                '''
            }
        }
    }
}
