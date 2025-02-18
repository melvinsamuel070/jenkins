
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
        EC2_SSH_KEY = credentials('ec2-ssh-key')
        INSTANCE_IP = credentials('ec2-instance-ip')
    }

    tools {
        nodejs 'node18'
    }
    stages {

        stage('Checkout') {
            steps {
                git branch: 'master', 
                url: 'https://github.com/melvinsamuel070/jenkins.git'
            }
        }

        stage('Starting') {
            steps {
                echo 'This is for the starting stage'
            }
        }

        stage('Login to DockerHub') {
            steps {
                script {
                    sh """
                       echo docker login -u \${DOCKERHUB_CREDENTIALS_USR} -p \${DOCKERHUB_CREDENTIALS_PSW} --password-stdin
                    """
                }
            }
        }
        stage('Building') {
            // // when {
            // //     expression {
            // //         BRANCH_NAME == 'testing'
            // //     }
            // }
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
                echo 'This is for the production stages'
            }
        }

        stage('Deploying') {
            steps {
                script {
                    sh """
                        chmod 600 private-key.pem'
                        ssh -o StrictHostKeyChecking=no -i private-key.pem ubuntu@${INSTANCE_IP} &&
                        sudo apt-get update &&
                        sudo apt-get install -y docker.io &&
                        sudo usermod -aG docker ubuntu &&
                        npm init -y &&
                        npm run test
                    '
                """
                }
            }      
            
        }
        stage('cloning the repo') {
            steps {
                sh """
                git branch: 'master', 
                url: 'https://github.com/melvinsamuel070/jenkins.git'
                cd jenkins
                """
            }
            
        }
    }

    post {
        failure {
            script {
                def build_log = readFile('builder.log')
                emailext subject: 'Everything FAILED',
                    body: """
                    This is the default body. ${env.JOB_NAME} - ${env.BUILD_NUMBER}, 
                    ${env.BUILD_URL}
                    ----------------
                    ${build_log}
                    """,
                    to: 'melvinsamuel070@gmail.com'
            }
        }

        success {
            script {
                def build_log = readFile('builder.log')
                emailext subject: 'Everything works fine from here',
                    body: """
                    This is the default body. ${env.JOB_NAME} - ${env.BUILD_NUMBER}, 
                    ${env.BUILD_URL}
                    ----------------
                    ${build_log}
                    """,
                    to: 'melvinsamuel070@gmail.com'
            }
        }
    }
}