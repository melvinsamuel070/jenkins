

pipeline {
    agent {label 'agent'}

    // environment {
    //     DOCKERHUB_CREDENTIALS = credentials('dockerhub')
    //     // EC2_SSH_KEY = credentials('ec2-ssh-key')
    //     // INSTANCE_IP = credentials('ec2-instance-ip')
    // }

    // // tools {
    // //     nodejs 'node18'
    // // }

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

        // stage('Login to DockerHub') {
        //     steps {
        //         script {
        //             sh """
        //                 echo docker login -u \${DOCKERHUB_CREDENTIALS_USR} -p \${DOCKERHUB_CREDENTIALS_PSW} --password-stdin
        //             """
        //         }
        //     }
        // }

        // stage('Building') {
        //     steps {
        //         sh """
        //           docker build -t melvinsamuel070/jenkins2 .
        //           docker push melvinsamuel070/jenkins2

        //         """
        //         script {
        //             try {
        //                 sh 'npm run test | tee builder.log'
        //             } catch (Exception err) {
        //                 currentBuild.result = 'FAILURE'
        //                 sh "echo ${err} | tee builder.log"
        //                 throw err
        //             }
        //         }
        //     }
        // }

        // stage('Production') {
        //     steps {
        //         script {
        //             sh """
        //                 docker pull melvinsamuel070/jenkins2:latest
        //                 npm install
        //                 npm update jest
        //                 npm install --save-dev jest
        //                 npm run test
        //             """
        //         }
        //     }
        // }
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
                                sudo apt-get update -y &&
                                sudo apt-get install -y docker.io nodejs npm &&
                                sudo usermod -aG docker ubuntu 
                                sudo apt update -y
                                sudo apt install fontconfig openjdk-17-jre -y
                                java -version
                                sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
                                https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
                                echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
                                https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
                               /etc/apt/sources.list.d/jenkins.list > /dev/null
                                sudo apt-get update -y
                                sudo apt-get install jenkins -y
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
                                git clone https://github.com/melvinsamuel070/jenkins.git jenkins
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
