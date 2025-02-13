



pipeline {
    agent any
    
    stages {
        stage("starting") {
            steps {
                echo "This is for the starting stage"
            }
        }

        stage("building") {
            steps {
                echo "This is for the building stage"
            }
        }

        stage("production") {
            steps {
                echo "This is for the production stage"
            }
        }

        stage("testing") {
            when {
                expression {
                    BRANCH_NAME == "testing"
                }
            }
            steps {
                echo "This is for the testing stage"
            }
        }
    }

    post {
        always {
            echo "This will always run"
        }
        success {
            mail to: 'melvinsamuel070@gmail.com',
                 subject: "SUCCESS: Pipeline Completed",
        }
        failure {
            mail to: 'melvinsamuel070@gmail.com',
                 subject: "FAILURE: Pipeline Failed",
                 body: "The pipeline has failed. Please check the logs."
        }
    }
}
