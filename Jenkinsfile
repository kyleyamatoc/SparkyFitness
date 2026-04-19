pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Install') {
            steps {
                bat 'corepack enable'
                bat 'pnpm -v'
                bat 'pnpm install --frozen-lockfile'
            }
        }
        stage('Build') {
            steps {
                bat 'pnpm -r build'
            }
        }
        stage('Test') {
            steps {
                bat 'pnpm --filter SparkyFitnessFrontend test'
                bat 'pnpm --filter SparkyFitnessServer exec vitest run --testTimeout=15000'
            }
        }
    }
    post {
        always {
            echo 'Cleaning up workspace'
            deleteDir()
        }
        success {
            echo 'Build succeeded!!!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}
