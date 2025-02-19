
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
        // EC2_SSH_KEY = credentials('ec2-ssh-key')
        // INSTANCE_IP = credentials('ec2-instance-ip')
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
            steps {
                sh """
                  docker build -t melvinsamuel070/jenkins2 .
                  docker push melvinsamuel070/jenkins2

                """
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
                script {
                    sh """
                        docker pull melvinsamuel070/jenkins2:latest
                        npm install
                        npm run test
                    """
                }
            }
        }

        /*
        stage('Deploying') {
            steps {
                script {
                    withCredentials([
                        sshUserPrivateKey(credentialsId: 'ec2-ssh-key', keyFileVariable: 'SSH_KEY_FILE'),
                        string(credentialsId: 'ec2-instance-ip', variable: 'INSTANCE_IP')
                    ]) {
                        writeFile file: 'main-pro.pem', text: readFile("${env.SSH_KEY_FILE}")
                        sh 'chmod 400 main-pro.pem'

                        sh """
                            ssh -o StrictHostKeyChecking=no -i main-pro.pem ubuntu@${INSTANCE_IP} '
                                sudo apt-get update &&
                                sudo apt-get install -y docker.io nodejs npm &&
                                sudo usermod -aG docker ubuntu 
                            '
                        """
                    }
                }
            }
        }

        stage('Cloning the Repo') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'ec2-instance-ip', variable: 'INSTANCE_IP')]) {
                        sh """
                            ssh -o StrictHostKeyChecking=no -i main-pro.pem ubuntu@${INSTANCE_IP} '
                                git clone https://github.com/melvinsamuel070/jenkins.git jenkins &&
                                cd jenkins
                            '
                        """
                    }
                }
            }
        }
        */
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
