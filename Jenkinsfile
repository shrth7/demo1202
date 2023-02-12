pipeline {
    agent any
    options{
        timestamps ()
        timeout(time: 100, unit: 'SECONDS')
         skipDefaultCheckout() 
         buildDiscarder(logRotator(numToKeepStr: '10'))
    }
    
    stages{
        stage("Checking versions"){
        parallel{
        stage('Docker Version'){
//             when{
//             branch "main"
//             }
            steps{
                sh 'docker --version'
            }
        }
        stage('GIT Version'){
//             when{
//             branch "main"
//             }
            steps{
                sh 'git --version'
            }
        }
    }
        }
        stage('Build Docker Image'){
//             when{
//             branch "main"
//             }

            steps{
                sh 'docker build -t imageagain${BUILD_NUMBER}:${BUILD_NUMBER} .'
                sh 'docker images'
//                 sh 'docker image inspect imageagain:18'
//                  sh 'docker kill $(docker ps -q)'
//                 sh 'docker rmi -f $(docker images -q)'
//                 sh 'docker rm $(docker ps -a -q)'
//                 sh 'docker container ls'
//                 sh 'docker images'
            }
        }
        stage('deploy') {
//             when{
//             branch "new_branch"
//             }
            steps{
            retry(3){
                
//                 sh 'docker run -d --name container${BUILD_NUMBER} imageagain44:44'
                sh 'docker container ls'
                }
            }
        }      
    }
}
