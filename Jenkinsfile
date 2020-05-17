        // PROJECT = "langues"
        // APP_NAME = "gceme"
        // FE_SVC_NAME = "${APP_NAME}-frontend"
        // IMAGE_TAG = "harbor.asaru.info/${PROJECT}/${APP_NAME}:${env.BRANCH_NAME}.${env.BUILD_NUMBER}"
        // JENKINS_CRED = "${PROJECT}"
        // JOB_NAME = "${JOB_NAME}"
        // DEPLOY_VERSION = "${DEPLOY_VERSION}"
        // BRANCH_NAME = "${BRANCH_NAME}"
        //String determineRepoName() { return scm.getUserRemoteConfigs()[0].getUrl().tokenize('/').last().split("\\.git")[0] }
pipeline {
    environment {
        registry = "harbor.asaru.info/langues/ng-app:1.1."
        dockerImage = ""
    }
    agent  { label 'jenkins-docker-nodejs' }
    stages {
        stage('Checkout Source') {
            steps {
                git 'https://github.com/IsuraGateways/ms-ng9.git'
            }
        }
        stage('Build image') {
            steps{
                container("jnlp") {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                    sh 'echo ==+==+===+===+=='
                }
            }
        }
        // stage ("Version Image"){
        //     steps {
        //         script {
        //             sh '''
        //                 docker tag chelibane/ng-app:$BUILD_NUMBER harbor.asaru.info/public-01/test-netflix:0.1.$BUILD_NUMBER
        //             '''
        //         }
        //     }
        // }
        stage('Publish Image') {
            steps {
                container("jnlp") {
                    withCredentials([usernamePassword(credentialsId: 'harbor', passwordVariable: 'p', usernameVariable: 'u')]) {
                        sh '''
                            docker login -u $u -p $p harbor.asaru.info
                            docker push harbor.asaru.info/langues/ng-app:1.1.$BUILD_NUMBER
                        '''
                    } 
                }
            }
        }
        stage ("Clean up") {
            steps {
                script {
                    sh '''
                        echo Cleanning up ...
                        docker image rm harbor.asaru.info/langues/ng-app:1.1.$BUILD_NUMBER 
                        docker images
                    '''
                }
            }
        }   
    }
}


// node {   
//     stage ("building docker image"){
//       sh '''
//         docker build -t ng-netflix .
//         docker images
//       '''
//     }
    
//     stage ("Taging docker Image"){
//       sh 'docker login -u Chelibane80 -p Tanina2008 harbor.asaru.info' 
//       sh 'docker tag ng-netfix:latest harbor.asaru.info/front-01/ng-netflix:0.0.1'
//     }
    
//     stage("Sending Image to Harbor"){

//       sh 'docker push harbor.asaru.info/front-01/ng-netflix:0.0.1'
//       sh 'docker image rm ng-netflix'
//     }
// }