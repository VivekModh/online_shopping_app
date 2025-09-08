pipeline {
    agent any

    environment {
        REGISTRY = "docker.io/vivek2good"
        IMAGE_NAME = "online_shop_app"
        BRANCH = "master"
    }
    stages {
        stage('Checkout Code') {
            steps {
                echo "🔄 Checking out branch ${BRANCH}..."
                git branch: "${BRANCH}", url: "https://github.com/VivekModh/online_shopping_app.git"
            }
        }

        stage('Install Dependencies & Build') {
            steps {
                echo "📦 Installing dependencies and building frontend..."
                sh """
                    npm ci
                    npm run build
                """
            }
        }
        stage('Security Scan') {
            steps {
                echo "🔍 Running Trivy scan..."
                sh """
                    trivy fs --exit-code 1 --severity CRITICAL,HIGH . 
                """
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "🐳 Building Docker image..."
                sh """
                    docker system prune -f   # Clean old images
                    docker build -t ${REGISTRY}/${IMAGE_NAME}:${BUILD_NUMBER} .
                    docker tag ${REGISTRY}/${IMAGE_NAME}:${BUILD_NUMBER} ${REGISTRY}/${IMAGE_NAME}:latest
                """
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerHubCred', 
                    usernameVariable: 'dockerHubUser', 
                    passwordVariable: 'dockerHubPass'
                )]) {
                    sh """
                        echo "$dockerHubPass" | docker login -u "$dockerHubUser" --password-stdin
                        docker tag ${REGISTRY}/${IMAGE_NAME}:${BUILD_NUMBER} ${dockerHubUser}/${IMAGE_NAME}:${BUILD_NUMBER}
                        docker tag ${REGISTRY}/${IMAGE_NAME}:${BUILD_NUMBER} ${dockerHubUser}/${IMAGE_NAME}:latest
                        docker push ${dockerHubUser}/${IMAGE_NAME}:${BUILD_NUMBER}
                        docker push ${dockerHubUser}/${IMAGE_NAME}:latest
                        docker logout
                    """
                }
            }
        }

        stage('Deploy') {
            steps {
                echo "🚀 Deploying application using Docker Compose..."
                sh """
                    docker-compose down || true
                    docker rm -f online_shop_app || true
                    docker-compose up -d --build
                """
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline completed successfully!"
        }
        failure {
            echo "❌ Pipeline failed! Check console output for details."
        }
        cleanup {
            echo "🧹 Cleaning up dangling Docker resources..."
            sh "docker system prune -f"
        }
    }
}


//DEVSECOPS PIPELINE
// pipeline {
//     agent any
    
//     environment {
//         REGISTRY     = "docker.io/vivek2good"
//         IMAGE_NAME   = "online_shop_app"
//         BRANCH       = "master"
//         SONAR_HOME   = tool "Sonar"
//     }

//     stages {
//         stage('Checkout Code') {
//             steps {
//                 echo "🔄 Checking out branch ${BRANCH}..."
//                 git branch: "${BRANCH}", url: "https://github.com/VivekModh/online_shopping_app.git"
//             }
//         }

//         stage('SonarQube Quality Analysis') {
//             steps {
//                 echo "🔍 Running SonarQube analysis..."
//                 withSonarQubeEnv("Sonar") {
//                     sh """
//                         $SONAR_HOME/bin/sonar-scanner \
//                         -Dsonar.projectName=${IMAGE_NAME} \
//                         -Dsonar.projectKey=${IMAGE_NAME}
//                     """
//                 }
//             }
//         }

//         stage('OWASP Dependency Check') {
//             steps {
//                 echo "🛡 Running OWASP Dependency Check..."
//                 dependencyCheck additionalArguments: '--scan ./', odcInstallation: 'dc'
//                 dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
//             }
//         }

//         stage("Sonar Quality Gate") {
//             steps {
//                 timeout(time: 2, unit: "MINUTES") {
//                     waitForQualityGate abortPipeline: false
//                 }
//             }
//         }

//         stage('Install Dependencies & Build') {
//             steps {
//                 echo "📦 Installing dependencies and building frontend..."
//                 sh """
//                     npm ci
//                     npm run build
//                 """
//             }
//         }

//         stage('Trivy File System Scan') {
//             steps {
//                 echo "🔍 Running Trivy FS scan..."
//                 sh "trivy fs --format table -o trivy-fs-report.html ."
//             }
//         }

//         stage('Build Docker Image') {
//             steps {
//                 echo "🐳 Building Docker image..."
//                 sh """
//                     docker system prune -f   # Clean old images
//                     docker build -t ${REGISTRY}/${IMAGE_NAME}:${BUILD_NUMBER} .
//                     docker tag ${REGISTRY}/${IMAGE_NAME}:${BUILD_NUMBER} ${REGISTRY}/${IMAGE_NAME}:latest
//                 """
//             }
//         }

//         stage('Trivy Image Scan') {
//             steps {
//                 echo "🔍 Scanning Docker image with Trivy..."
//                 sh "trivy image ${REGISTRY}/${IMAGE_NAME}:${BUILD_NUMBER} --exit-code 0 --severity HIGH,CRITICAL"
//             }
//         }

//         stage('Push Docker Image') {
//             steps {
//                 withCredentials([usernamePassword(
//                     credentialsId: 'dockerHubCred', 
//                     usernameVariable: 'dockerHubUser', 
//                     passwordVariable: 'dockerHubPass'
//                 )]) {
//                     sh """
//                         echo "$dockerHubPass" | docker login -u "$dockerHubUser" --password-stdin
//                         docker tag ${REGISTRY}/${IMAGE_NAME}:${BUILD_NUMBER} ${dockerHubUser}/${IMAGE_NAME}:${BUILD_NUMBER}
//                         docker tag ${REGISTRY}/${IMAGE_NAME}:${BUILD_NUMBER} ${dockerHubUser}/${IMAGE_NAME}:latest
//                         docker push ${dockerHubUser}/${IMAGE_NAME}:${BUILD_NUMBER}
//                         docker push ${dockerHubUser}/${IMAGE_NAME}:latest
//                         docker logout
//                     """
//                 }
//             }
//         }

//         stage('Deploy') {
//             steps {
//                 echo "🚀 Deploying application using Docker Compose..."
//                 sh """
//                     docker-compose down || true
//                     docker rm -f online_shop_app || true
//                     docker-compose up -d --build
//                 """
//             }
//         }
//     }

//     post {
//         success {
//             echo "✅ DevSecOps pipeline completed successfully!"
//         }
//         failure {
//             echo "❌ Pipeline failed! Check console output for details."
//         }
//         cleanup {
//             echo "🧹 Cleaning up dangling Docker resources..."
//             sh "docker system prune -f"
//         }
//     }
// }
