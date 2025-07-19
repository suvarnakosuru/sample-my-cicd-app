// Jenkinsfile
pipeline {
    agent any // The agent specifies where the pipeline will run. 'any' means any available agent.

    environment {
        // Define environment variables specific to your project
        // For example, the build tool command
        // MAVEN_HOME = tool 'Maven 3.8.6' // If you've configured Maven in Jenkins Global Tool Configuration
        DEPLOY_DIR = '/d/CI_CD_pipeline/DevOps/my-cicd-app/opt/my-app-deploy' // Target deployment directory on localhost
        SOURCE_CODE_DIR = '/d/CI_CD_pipeline/DevOps/my-cicd-app/' // Name of your project directory after cloning
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    echo 'Cloning the repository...'
                    // Clean workspace before checkout to ensure a fresh build
                    cleanWs()
                    // Checkout the source code from the configured SCM (Git)
                    // IMPORTANT: Replace '/path/to/your/local/git/repo.git' with the actual path to your Git repository
                    git branch: 'main', url: 'https://github.com/suvarnakosuru/sample-my-cicd-app.git'
                    // Or if using the SCM configured in the job:
                    // checkout scm
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    echo 'Building the application...'
                    // Example for a Maven project:
                    // Assuming your project is in a subdirectory named 'my-sample-app'
                    dir("${SOURCE_CODE_DIR}") {
                        sh 'mvn clean package' // Command to build your application
                    }
                    // For a Node.js project:
                    // sh 'npm install'
                    // sh 'npm run build'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    echo 'Running tests...'
                    // Example for a Maven project:
                    dir("${SOURCE_CODE_DIR}") {
                        sh 'mvn test' // Command to run tests
                    }
                    // For a Node.js project:
                    // sh 'npm test'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo "Deploying to ${DEPLOY_DIR}..."
                    // Create the deployment directory if it doesn't exist
                    sh "mkdir -p ${DEPLOY_DIR}"

                    // Copy the built artifact to the deployment directory
                    // Example for a Maven .jar or .war file:
                    // Assuming the artifact is in target/ after build
                    def artifactPath = "${SOURCE_CODE_DIR}/target/*.jar" // Adjust for .war or other extensions
                    sh "cp ${artifactPath} ${DEPLOY_DIR}/"

                    // For a Node.js project, you might copy the entire build directory:
                    // sh "cp -R ${SOURCE_CODE_DIR}/build/* ${DEPLOY_DIR}/"

                    echo "Deployment complete. Artifact copied to ${DEPLOY_DIR}"
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
        unstable {
            echo 'Pipeline is unstable.'
        }
    }
}
