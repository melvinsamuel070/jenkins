



pipeline {
    agent any
    
    stages {
        stage("starting") {
            steps {
                echo "This is for the starting stags"
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
        steps {
            script {
                try {
                    sh "ssh -o -i tester.key root@123.222.33.44" 
                    
                } catch (error) {
                    currentBuild.result = "FAILURE"
                    throw err
                }
                    
            }
        }
    }

    post {
        always {
            echo "This will always run"
        }
        failure {
            script {
                def build_log = Mnager.build.logs
                emailext subject: "All FAILED",
                         body: """
                                this is bthe default body
                                ${env.BUILD_URL}
                                ----------------
                                ${build_log}
                                """,
                         to: "melvinsamuel070@gmail.com"

            }
        }
        success {
            emailext subject: "All SUCCESS",
                         body: """
                                this is bthe default body
                                ${env.BUILD_URL}
                                ----------------
                                ${build_log}
                                """,
                         to: "melvinsamuel070@gmail.com"
        }

    }
}