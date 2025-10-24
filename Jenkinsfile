pipeline {
    agent any

    environment {
        REPORT_DIR = 'reports'
    }

    stages {
        stage('Prepare Workspace') {
            steps {
                sh "mkdir -p ${REPORT_DIR}"
            }
        }

       stage('Build & Run Suites') {
    steps {
        sh 'docker-compose up --build --abort-on-container-exit'
    }
}

stage('Copy Reports') {
    steps {
        sh '''
            cp reports/regression.html reports/regression.html || echo "Regression report not found"
            cp reports/parametrize.html reports/parametrize.html || echo "Parametrize report not found"
            cp reports/sanity.html reports/sanity.html || echo "Sanity report not found"
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
                archiveArtifacts artifacts: 'reports/*.html', allowEmptyArchive: true
            }
        }

        stage('Clean Up Containers') {
            steps {
                sh 'docker-compose down'
            }
        }
    }
}
