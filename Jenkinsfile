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
//                 success {
//                     echo "Job was a success"
//                 }
//                 failure {
//                     echo "Job was a failure"
//                 }
//             }
//         }
//     }
// }

pipeline {
    agent any
        tools {
            nodejs "node18"
    }

     stages {
        // stage('checkout') {
            // checkout all files
        //     steps{
        //          git "https://github.com/Oluwaseun186/jenkinsfile.git"
        //     }     
        // }

        stage('build') {

            // PASSING BRANCH NAME AS A CONDITION
             when{
                 expression{
                    BRANCH_NAME == "master."
                 }
             }
            steps {

                echo "this is building step."
                // RUNNING NPM INSTALL AND TESTING WHETHER THE INSTALLTION ACHIEVED
                // script{
                //     try{
                //         sh 'touch build.log'
                //         sh "npm install"
                //         sh "npm run build"
                //         sh "npm run test"
                       
                //     }catch(Exception err){
                //         echo "error is ${err.getMessage}"
                //         throw err
                //     }
                }
            }
        stage('Deploy') {

            steps {
                echo "this is building step."
            }
        }
      
    }

    // POST BUILD FOR FAILURE AND SUCCESS OF RUN JOBS
    post {
        always {
            echo "this is just a step.."
        }
        success {
            emailext(
                subject: "Build ${currentBuild.currentResult}: Job ${env.JOB_NAME}, ${BUILD_NUMBER}  ${JOB_NAME},${env.BUILD_LOG}, ${env.BUILD_URL}",
                body: "Build Status: ${currentBuild.currentResult}\nCheck the console output at ${env.BUILD_URL}",
                to: "kingsamuel412@gmail.com",
                replyTo: "kingsamuel412@gmail.com",
                from: "melvinsamuel070@gmail.com"
            )
        }
        failure {
                script{
                     //def build_log = currentBuild.rawBuild.getLog(100).join('\n') 
                     //def build_log = Manager.build.log
                     def build_log = readFile("build.log")
                        emailext(
                            subject: "Build ${currentBuild.currentResult}: Job ${env.JOB_NAME}, ${env.BUILD_NUMBER}, ${JOB_NAME}, ${build_log},  ${BUILD_URL}",
                            body: "Build Status: ${currentBuild.currentResult}\nCheck the console output at ${env.BUILD_URL}, ${build_log}, ${env.BUILD_NUMBER}",
                            to: "kingsamuel412@gmail.com",
                            replyTo: "kingsamuel412@gmail.com",
                            from: "melvinsamuel070@gmail.com"
                        )                    
                }
        }
    }
}