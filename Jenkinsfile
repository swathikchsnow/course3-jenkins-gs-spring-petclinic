pipeline {
    agent any
    
    stages {
        // stage("checkout") {
        //     steps {
        //         script {
        //             echo "Checking out code"
        //             git branch: 'main', url: 'https://github.com/swathikchsnow/course3-jenkins-gs-spring-petclinic.git'
        //             echo "List files after checkout"
        //             sh "ls"
        //         }
        //     }
        // }
        
        stage("build") {
            steps {
                echo "Building the project"
                sh "./mvnw package"
            }
        }

        stage("tests") {
            parallel {
                stage("tests A") {
                    steps {
                        echo "Running test set A"
                        sh "echo test set A"
                        sleep 2
                        sleep 3
                    }
                }
                stage("tests B") {
                    steps {
                        echo "Running test set B"
                        sh "echo test set B"
                        sleep 4
                    }
                }
                stage("tests C") {
                    steps {
                        echo "Running test set C"
                        sleep 1
                        //sh "false"
                    }
                }
            }
        }

        stage("capture") {
            steps {
                echo "Archiving artifacts"
                archiveArtifacts '**/target/*.jar'
                jacoco()
                junit '**/target/surefire-reports/TEST*.xml'
            }
        }
    }

    post {
        always {
            echo "Sending email notification"
        }
        failure {
            emailext body: "${env.BUILD_URL} \n ${currentBuild.absoluteUrl}",
            to: 'swathi.chadalavada@servicenow.com',
            recipientProviders: [culprits(), developers()],
            subject: "${currentBuild.currentResult}: Job ${env.JOB_NAME} [${env.BUILD_NUMBER}]"
        }
    }
}

