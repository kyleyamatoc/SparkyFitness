pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Use the Jenkins job's SCM configuration (recommended). If you prefer explicit checkout,
                // replace this with a git(...) step pointing at your repo.
                checkout scm
            }
        }

        stage('Install') {
            steps {
                // Repo is a pnpm workspace (see pnpm-workspace.yaml)
                // Use corepack so pnpm is available on the agent.
                bat 'corepack enable'
                bat 'pnpm -v'
                bat 'pnpm install --frozen-lockfile'
            }
        }

        stage('Build') {
            steps {
                // Build all workspace packages (frontend, SparkyFitnessFrontend, shared,
                // SparkyFitnessMobile, SparkyFitnessServer, docs)
                bat 'pnpm -r build'
            }
        }

        stage('Test') {
            steps {
                // Run tests across the workspace (if packages define a test script)
                bat 'pnpm -r test'
            }
        }
    }

    post {
        always {
            echo 'Cleaning up workspace'
            deleteDir() // Clean up the workspace after the build
        }
        success {
            echo 'Build succeeded!!!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}