pipeline {
    agent any

    environment {
        REPORT_DIR = 'reports'
    }

    stages {
        stage('Prepare Workspace') {
            steps {
                // Ensure the reports directory exists before mounting
                sh "mkdir -p ${REPORT_DIR}"
            }
        }
        stage('Build & Run Suites') {
            steps {
                sh 'docker-compose up --build'
            }
        }

        stage('Copy Reports') {
            steps {
                sh '''
                    docker cp regression_runner:/app/regression.html reports/regression.html
                    docker cp sanity_runner:/app/sanity.html reports/sanity.html
                    docker cp system_runner:/app/parametrize.html reports/parametrize.html
                '''
            }
        }

        stage('Send Email') {
            steps {
                emailext (
                    subject: "Pytest Report",
                    body: "Test execution completed. Please find the attached HTML reports.",
                    to: "deepthi1987.p@gmail.com",
                    attachLog: false,
                    attachmentsPattern: "reports/*.html"
                )
            }
        }

        stage('Archive Reports') {
            steps {
                archiveArtifacts artifacts: 'reports/*.html', allowEmptyArchive: true  //collects all HTML reports from the shared reports/ folder
            }
        }
        stage('Clean Up Containers') {
            steps {
                // Stop and remove containers after test execution
                sh 'docker-compose down'
            }
        }
    }
    }
}
