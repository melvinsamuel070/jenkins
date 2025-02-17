
// pipeline {
//     agent any
    
//     tools {
//         nodejs "node:18"
//     }

//     stages {

//         stage("Checkout out") {
//             steps {
//                 git "https://github.com/melvinsamuel070/jenkins.git"
//             }
//         }

//         stage("starting") {
//             steps {
//                 echo "This is for the starting stage"
//             }
//         }

//         stage("building") {
//             when {
//                 expression {
//                     BRANCH_NAME == "testing"
//                 }
//             }
//             steps {
//                 script {
//                     try {
//                         sh "npm run test | tee build.log"
//                     } catch (Exception err) {
//                         currentBuild.result = "FAILURE"
//                         throw err
//                     }
//                 }
//             }
//         }

//         stage("production") {
//             steps {
//                 echo "This is fo the prduction stage"
//             }
//         }
//     }

//    post {
//     always {
//         echo "This will always run"
//     }

//     failure {
//         script {
//             def build_log = currentBuild.rawBuild.getLog(50).join("\n")
//             emailext subject: "Build FAILED",
//                      body: """
//                          Build failed for ${env.JOB_NAME} #${env.BUILD_NUMBER}
//                          URL: ${env.BUILD_URL}
//                          Logs:
//                          ${build_log}
//                      """,
//                      to: 'melvinsamuel070@gmail.com'
//         }
//     }

//     success {
//         emailext subject: "Build SUCCESS",
//                  body: """
//                      Build succeeded for ${env.JOB_NAME} #${env.BUILD_NUMBER}
//                      URL: ${env.BUILD_URL}
//                  """,
//                  to: 'melvinsamuel070@gmail.com'
//     }
// }

// }


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





pipeline {
    agent any

    tools {
        nodejs "node:18"
    }

    stages {
        stage("checkout") {
            steps {
                git "https://github.com/melvinsamuel070/jenkins.git"
            }
        }

        stage("starting") {
            steps {
                echo "This is the starting stage"
            }
        }

        stage("Build") {
            when {
                expression {
                    env.BRANCH_NAME == "testing"  
                }
            }
            steps {
                script {
                    try {
                        sh "npm run test | tee build.log"
                    } catch (err) {
                        echo "error is ${err.getMessage}"
                        throw err
                    }
                }
            }
        }
    }

    post {
        // success {
        //     emailext(
        //         subject: "Build ${currentBuild.currentResult}: Job ${env.JOB_NAME} #${BUILD_NUMBER}",
        //         body: "Build Status: ${currentBuild.currentResult}\nCheck the console output at ${env.BUILD_URL}",
        //         to: "oladapper92@gmail.com",
        //         replyTo: "oladapper92@gmail.com",
        //         from: "oladapper@gmail.com"
        //     )
        // }
        failure {
            script {
                def build_log = readFile("build.log")  // ✅ Reads log file for failure email
                emailext(
                    subject: "Build Failed: ${env.JOB_NAME} #${BUILD_NUMBER}",
                    body: """Build Status: ${currentBuild.currentResult}
                    Check the console output at: ${env.BUILD_URL}
                    Last 100 lines of build log:
                    ${build_log}
                    """,
                    to: "melvinsamuel070@gmail.com",
                    replyTo: "melvinsamuel070@gmail.com",
                    from: "melvinsamuel070@gmail.com"
                )
            }
        }
    }
}