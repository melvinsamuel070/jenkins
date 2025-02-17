
pipeline {
    agent any
    
    tools {
        nodejs "node:18"
    }

    stages {

        stage("Checkout out") {
            steps {
                git "https://github.com/melvinsamuel070/jenkins.git"
            }
        }

        stage("starting") {
            steps {
                echo "This is for the starting stage"
            }
        }

        stage("building") {
            when {
                expression {
                    BRANCH_NAME == "testing"
                }
            }
            steps {
                script {
                    try {
                        sh "npm run test | tee build.log"
                    } catch (Exception err) {
                        currentBuild.result = "FAILURE"
                        throw err
                    }
                }
            }
        }

        stage("production") {
            steps {
                echo "This is for the prduction stage"
            }
        }
    }

   post {
    always {
        echo "This will always run"
    }

    failure {
        script {
            def build_log = currentBuild.rawBuild.getLog(50).join("\n")
            emailext subject: "Build FAILED",
                     body: """
                         Build failed for ${env.JOB_NAME} #${env.BUILD_NUMBER}
                         URL: ${env.BUILD_URL}
                         Logs:
                         ${build_log}
                     """,
                     to: 'melvinsamuel070@gmail.com'
        }
    }

    success {
        emailext subject: "Build SUCCESS",
                 body: """
                     Build succeeded for ${env.JOB_NAME} #${env.BUILD_NUMBER}
                     URL: ${env.BUILD_URL}
                 """,
                 to: 'melvinsamuel070@gmail.com'
    }
}

}


//       post{
//             failure{
//                 script {
//                     //def build_log = currentBuild.rawBuild.getLog(200).join("\n"),
//                     // def build_log = manager.build.log
//                     def build_log = readFile("build.log")
//                     emailext subject: "Everything FAILED",
//                             body: """
//                                     This is the default body. ${env.JOB_NAME} - ${env.BUILD_NUMBER}, 
//                                     ${env.BUILD_URL}
//                                     ----------------
//                                     ${build_log}
//                                     """,
//                             to: "melvinsamuel070@gmail.com"

//                 }
                
//             }

//              success{
//                 emailext subject: "Everything works fine from here",
//                          body: """
//                                 This is the default body. ${env.JOB_NAME} - ${env.BUILD_NUMBER}, 
//                                 ${env.BUILD_URL}
//                                 ----------------
                            
//                                 """
//                          to: "melvinsamuel070@gmail.com"
                
//             }
//         }
// }