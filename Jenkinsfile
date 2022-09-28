// ----------FIRST JENKINS PIPELINE DEMO----------

// pipeline {
//     agent any
//     tools {
//   maven 'M2_HOME'
// }
//     triggers {
//  pollSCM('* * * * *')
//     }
    
//     stages {
//         stage('Build') {
//             steps {
//                 sh 'mvn clean'
//                 sh 'mvn install'
//                 sh 'mvn package'
//             }
//         }
//           stage('Test') {
//             steps {
//                 sh 'mvn test'
//             }
//         }
//           stage('Deploy') {
//             steps {
//                 echo 'Deploy Step'
//             }
//         }
//           stage('Docker') {
//             steps {
//                 echo 'Image Step'
//             }
//         }
//     }
// }

// // Poll SCM works!!

// ----------FIRST JENKINS PIPELINE PROJECT----------
pipeline {
    agent any
    tools{
        maven 'M2_HOME'
    }
    environment {
    registry = '076892551558.dkr.ecr.us-east-1.amazonaws.com/devop_repository'
    registryCredential = 'jenkins-ecr'
    dockerimage = ''
  }
    stages {
        stage('Checkout'){
            steps{
                git branch: 'main', url: 'https://github.com/Hermann90/helloworld_jan_22.git'
            }
        }
        stage('Code Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Build Image') {
            steps {
                script{
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                } 
            }
        }
        stage('Deploy image') {
            steps{
                script{ 
                    docker.withRegistry("https://"+registry,"ecr:us-east-1:"+registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        }  
    }
}