pipeline {
    agent any
    parameters {
        choice choices: ['chrome', 'firefox'], description: 'Select the browser', name: 'BROWSER'
    }
    stages {
        stage('Start the Grid') {
            steps {
                sh "docker-compose -f grid.yaml up --scale ${params.BROWSER}=2 -d"
            }
        }
        stage('Run tests') {
            steps {
                sh "docker-compose -f test-suites.yaml up --pull=always"
            }
            script {
                // if there are test failures, mark the Jenkins build as failed
                if (fileExists('output/flight-reservation/testng-failed.xml') || fileExists('output/vendor-portal/testng-failed.xml')) {
                    error("Test failures detected.")
                }
            }
        }
    }
    post {
        always {
            sh "docker-compose -f grid.yaml down"
            sh "docker-compose -f test-suites.yaml down"
            archiveArtifacts artifacts: 'output/vendor-portal/emailable-report.html', followSymlinks: false
            archiveArtifacts artifacts: 'output/flight-reservation/emailable-report.html', followSymlinks: false
        }
    }
}
