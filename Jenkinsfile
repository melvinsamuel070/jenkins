
pipeline {
    agent any
    tools {
        nodejs "node18"
    }
    stages {

        stage("Checkout out"){
            git "https://github.com/melvinsamuel070/jenkins.git"
        }

        stage("starting"){
            steps{
                echo "This is for the starting stage"
            }
        }


         stage("building"){
            when{
                expression{
                    BRANCH_NAME == "testing"
                }
            }
            steps{
                script {
                    try {

                        sh "npm run test | tee build.log"

                    }catch(Exception err){
                        currentBuild.result = "FAILURE"

                        throw err
                    }
                }
            }
        }

         stage("production"){
            steps{
                echo "This is for the prduction stage"
            }
        }

    }

      post{
            failure{
                script {
                    //def build_log = currentBuild.rawBuild.getLog(200).join("\n"),
                    // def build_log = manager.build.log
                    def build_log = readFile("build.log")
                    emailext subject: "Everything FAILED",
                            body: """
                                    This is the default body. ${env.JOB_NAME} - ${env.BUILD_NUMBER}, 
                                    ${env.BUILD_URL}
                                    ----------------
                                    ${build_log}
                                    """,
                            to: "melvinsamuel070@gmail.com"

                }
                
            }

             success{
                emailext subject: "Everything works fine from here",
                         body: """
                                This is the default body. ${env.JOB_NAME} - ${env.BUILD_NUMBER}, 
                                ${env.BUILD_URL}
                                ----------------
                            
                                """,
                         to: "melvinsamuel070@gmail.com"
                
            }
        }
}