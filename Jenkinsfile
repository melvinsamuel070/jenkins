
pipeline {
    agent any
    tools {
        nodejs "node18"
    }
    stages {

        stage("Checkout out"){
            steps{
                git branch: "master", 
                url: "https://github.com/melvinsamuel070/jenkins.git"
            }
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
                 sh 'touch builder.log'  // Ensure log file exists
                 sh 'npm install > builder.log 2>&1'
                 sh 'npm run build >> builder.log 2>&1'
        
                script {
                    try {

                        sh "npm run test | tee builder.log"

                    }catch(Exception err){
                        currentBuild.result = "FAILURE"
                        sh "echo ${err} | tee builder.log"
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
                    //def build_log = currentBuil.rawBuild.getLog(200).join("\n")
                    // def build_log = manager.build.log
                    def build_log = readFile("builder.log")
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

                    script {
                        def build_log = readFile("builder.log")
                        emailext subject: "Everything works fine from her",
                        body: """
                                This is the default body. ${env.JOB_NAME} - ${env.BUILD_NUMBER}, 
                                ${env.BUILD_URL}
                                ----------------
                                ${build_log}
                                """,
                        to: "melvinsamuel070@gmail.com"

                    }
               
                
            }
        }
}