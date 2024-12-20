pipeline {
    agent {
        docker {
            image 'node:16-alpine' // Use a more recent Node.js version compatible with React
            args '-p 3000:3000' // Maps port 3000 of the container to the host
        }
    }
    environment {
        CI = 'true' // Indicates a CI environment for npm
    }
    stages {
        stage('Install Dependencies') {
            steps {
                echo 'Installing dependencies...'
                sh 'npm install' // Installs project dependencies
            }
        }
        stage('Build React App') {
            steps {
                echo 'Building the React application...'
                sh 'npm run build' // Builds the React app for production
            }
        }
        stage('Test React App') {
            steps {
                echo 'Running tests...'
                sh './jenkins/scripts/test.sh' // Executes the test script
            }
        }
        stage('Serve React App') {
            steps {
                echo 'Serving the React application...'
                sh 'npm install -g serve' // Installs the "serve" package globally
                sh 'serve -s build -l 3000 &' // Serves the "build" folder on port 3000
            }
        }
        stage('User Confirmation') {
            steps {
                input message: 'Finished using the website? (Click "Proceed" to continue)'
            }
        }
        stage('Cleanup') {
            steps {
                echo 'Stopping the server...'
                sh 'pkill -f serve || true' // Kills the "serve" process to stop the server
            }
        }
    }
}
