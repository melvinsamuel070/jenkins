
// pipeline {
//     agent any
//     tools {
//         nodejs "node18"
//     }
//     stages {

//         stage("Checkout out"){
//             steps{
//                 git branch: "master", 
//                 url: "https://github.com/melvinsamuel070/jenkins.git"
//             }
//         }

//         stage("starting"){
//             steps{
//                 echo "This is for the starting stage"
//             }
//         }


//          stage("building"){
//             when{
//                 expression{
//                     BRANCH_NAME == "testing"
//                 }
//             }
//             steps{
        
//                 script {
//                     try {

//                         sh "npm run test | tee builder.log"

//                     }catch(Exception err){
//                         currentBuild.result = "FAILURE"
//                         sh "echo ${err} | tee builder.log"
//                         throw err
//                     }
//                 }
//             }
//         }

//          stage("production"){
//             steps{
//                 echo "This is for the prduction stage"
//             }
//         }

//     }

//       post{
//             failure{
//                 script {
//                     //def build_log = currentBuil.rawBuild.getLog(200).join("\n")
//                     // def build_log = manager.build.log
//                     def build_log = readFile("builder.log")
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

//                     script {
//                         def build_log = readFile("builder.log")
//                         emailext subject: "Everything works fine from her",
//                         body: """
//                                 This is the default body. ${env.JOB_NAME} - ${env.BUILD_NUMBER}, 
//                                 ${env.BUILD_URL}
//                                 ----------------
//                                 ${build_log}
//                                 """,
//                         to: "melvinsamuel070@gmail.com"

//                     }
               
                
//             }
//         }
// }



pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')
    }
    tools {
        nodejs 'node18'
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/melvinsamuel070/jenkins.git'
            }
        }

        stage('Starting') {
            steps {
                echo 'This is for the starting stage'
            }
        }

        stage('Logging to Docker') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                    }
                }
            }
        }

        stage('Building') {
            when {
                expression { env.BRANCH_NAME == 'testing' }
            }
            steps {
                sh 'docker build -t melvinsamuel070/jenkins .'
                script {
                    try {
                        sh 'npm run test | tee builder.log'
                    } catch (Exception err) {
                        currentBuild.result = 'FAILURE'
                        sh "echo ${err} | tee builder.log"
                        throw err
                    }
                }
            }
        }

        stage('Production') {
            steps {
                echo 'This is for the production stage'
            }
        }
    }

    post {
        failure {
            script {
                def build_log = readFile('builder.log')
                emailext subject: 'Build FAILED: ${env.JOB_NAME} #${env.BUILD_NUMBER}',
                         body: """
                             Build failed for ${env.JOB_NAME} #${env.BUILD_NUMBER}:
                             ${env.BUILD_URL}

                             Logs:
                             ----------------
                             ${build_log}
                         """,
                         to: 'melvinsamuel070@gmail.com'
            }
        }

        success {
            script {
                def build_log = readFile('builder.log')
                emailext subject: 'Build SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}',
                         body: """
                             Build succeeded for ${env.JOB_NAME} #${env.BUILD_NUMBER}:
                             ${env.BUILD_URL}

                             Logs:
                             ----------------
                             ${build_log}
                         """,
                         to: 'melvinsamuel070@gmail.com'
            }
        }
    }
}
