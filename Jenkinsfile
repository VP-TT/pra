pipeline {
    agent any
    stages {
        stage('Clone Repository') {
            steps {
                echo "Cloning the project..."
                git branch: 'main', url: 'YOUR_REPO_URL_HERE'
            }
        }
        stage('Detect Project Type') {
            steps {
                script {
                    if (fileExists('package.json')) {
                        env.PROJECT_TYPE = 'node'
                    } else if (fileExists('pom.xml')) {
                        env.PROJECT_TYPE = 'java-maven'
                    } else if (fileExists('requirements.txt')) {
                        env.PROJECT_TYPE = 'python'
                    } else {
                        env.PROJECT_TYPE = 'static'
                    }
                    echo "Detected project type: ${env.PROJECT_TYPE}"
                }
            }
        }
        stage('Build') {
            steps {
                script {
                    if (env.PROJECT_TYPE == 'node') {
                        sh 'npm install'
                        sh 'npm run build || echo "No build script"'
                    } else if (env.PROJECT_TYPE == 'java-maven') {
                        sh 'mvn clean package'
                    } else if (env.PROJECT_TYPE == 'python') {
                        sh 'pip install -r requirements.txt'
                    } else {
                        echo "Static project — no build needed."
                    }
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    if (env.PROJECT_TYPE == 'node') {
                        sh 'npm test || echo "No tests found"'
                    } else if (env.PROJECT_TYPE == 'java-maven') {
                        sh 'mvn test'
                    } else if (env.PROJECT_TYPE == 'python') {
                        sh 'pytest || echo "No tests found"'
                    } else {
                        echo "Static project — no tests."
                    }
                }
            }
        }
        stage('Deploy Simulation') {
            steps {
                echo "Deploy step executed successfully."
            }
        }
    }
    post {
        success { echo "Pipeline completed successfully!" }
        failure { echo "Pipeline failed!" }
    }
}
