pipeline {
    agent any
    environment {
        USER_CUSTOM_VARIABLE = "samuel"
        ANOTHER_CUSTOM_VARIABLE = "song"
    }
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
            post {
                always {
                    echo "Finished job"
                }
                success {
                    echo "Job was a success"
                }
                failure {
                    echo "Job was a failure"
                }
            }
        }
    }
}