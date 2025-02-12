// pipeline {
//     agent any
//     environment {
//         USER_CUSTOM_VARIABLE = "samuel"
//         ANOTHER_CUSTOM_VARIABLE = "song"
//     }
//     stages {
//         stage("starting") {
//             steps {
//                 echo "This is for the starting stage"
//             }
//         }

//         stage("building") {
//             steps {
//                 echo "This is for the building stage"
//             }
//         }

//         stage("production") {
//             steps {
//                 echo "This is for the production stage"
//             }
//         }

//         stage("testing") {
//             when {
//                 expression {
//                     BRANCH_NAME == "testing"
//                 }
//             }
//             steps {
//                 echo "This is for the testing stage"
//             }
//             post {
//                 always {
//                     echo "Finished job"
//                 }
               
               
//             }
//         }
//     }
// }





// pipeline {
//     agent any
    
//     environment {
//         // SMTP Configuration
//         SMTP_SERVER = "smtp.gmail.com"
//         SMTP_PORT = "587"
//         EMAIL_RECIPIENT = "recipient@example.com" // Replace with the recipient email
//         SMTP_CREDENTIALS = credentials('smtp-credentials') // Retrieve SMTP credentials
//     }
    
//     stages {
//         stage("starting") {
//             steps {
//                 echo "This is for the starting stage"
//             }
//         }

//         stage("building") {
//             steps {
//                 echo "This is for the building stage"
//             }
//         }

//         stage("production") {
//             steps {
//                 echo "This is for the production stage"
//             }
//         }

//         stage("testing") {
//             when {
//                 expression {
//                     BRANCH_NAME == "testing"
//                 }
//             }
//             steps {
//                 echo "This is for the testing stage"
//             }
//         }
//     }

//     post {
//         success {
//             script {
//                 // Extract username and password from credentials
//                 def smtpUser = SMTP_CREDENTIALS_USR
//                 def smtpPassword = SMTP_CREDENTIALS_PSW

//                 // Send email using emailext plugin
//                 emailext (
//                     subject: "SUCCESS: Pipeline Completed",
//                     body: "The pipeline has completed successfully.",
//                     to: "${EMAIL_RECIPIENT}",
//                     smtpServer: "${SMTP_SERVER}",
//                     smtpPort: "${SMTP_PORT}",
//                     smtpUsername: "${smtpUser}",
//                     smtpPassword: "${smtpPassword}",
//                     useTLS: true
//                 )
//             }
//         }
//         failure {
//             script {
//                 // Extract username and password from credentials
//                 def smtpUser = SMTP_CREDENTIALS_USR
//                 def smtpPassword = SMTP_CREDENTIALS_PSW

//                 // Send email using emailext plugin
//                 emailext (
//                     subject: "FAILURE: Pipeline Failed",
//                     body: "The pipeline has failed. Please check the logs.",
//                     to: "${EMAIL_RECIPIENT}",
//                     smtpServer: "${SMTP_SERVER}",
//                     smtpPort: "${SMTP_PORT}",
//                     smtpUsername: "${smtpUser}",
//                     smtpPassword: "${smtpPassword}",
//                     useTLS: true
//                 )
//             }
//         }
//     }
// }





// pipeline {
//     agent any
    
//     environment {
//         // SMTP Configuration
//         SMTP_SERVER = "smtp.gmail.com"
//         SMTP_PORT = "587"
//         EMAIL_RECIPIENT = "melvinsamuel070@gmailcom" // Replace with the recipient email
//         SMTP_USER = credentials('smtp-username') // Retrieve SMTP username from Jenkins credentials
//         SMTP_PASSWORD = credentials('smtp-password') // Retrieve SMTP password from Jenkins credentials
//     }
    
//     stages {
//         stage("starting") {
//             steps {
//                 echo "This is for the starting stage"
//             }
//         }

//         stage("building") {
//             steps {
//                 echo "This is for the building stage"
//             }
//         }

//         stage("production") {
//             steps {
//                 echo "This is for the production stage"
//             }
//         }

//         stage("testing") {
//             when {
//                 expression {
//                     BRANCH_NAME == "testing"
//                 }
//             }
//             steps {
//                 echo "This is for the testing stage"
//             }
//         }
//     }

//     post {
//         always {
//             echo "This block will always run, regardless of success or failure."
//             // Add any cleanup or logging tasks here
//         }
//         success {
//             emailext (
//                 subject: "SUCCESS: Pipeline Completed",
//                 body: "The pipeline has completed successfully.",
//                 to: "${EMAIL_RECIPIENT}",
//                 smtpServer: "${SMTP_SERVER}",
//                 smtpPort: "${SMTP_PORT}",
//                 smtpUsername: "${SMTP_USER}",
//                 smtpPassword: "${SMTP_PASSWORD}",
//                 useTLS: true
//             )
//         }
//         failure {
//             emailext (
//                 subject: "FAILURE: Pipeline Failed",
//                 body: "The pipeline has failed. Please check the logs.",
//                 to: "${EMAIL_RECIPIENT}",
//                 smtpServer: "${SMTP_SERVER}",
//                 smtpPort: "${SMTP_PORT}",
//                 smtpUsername: "${SMTP_USER}",
//                 smtpPassword: "${SMTP_PASSWORD}",
//                 useTLS: true
//             )
//         }
//     }
// }









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
        success {
            mail to: 'melvinsamuel070@gmail.com', // Replace with your email
                 subject: "SUCCESS: Pipeline Completed",
                 body: "The pipeline has completed successfully."
        }
        failure {
            mail to: "@gmail.com', // Replace with your email
                 subject: "FAILURE: Pipeline Failed",
                 body: "The pipeline has failed. Please check the logs."
        }
    }
}