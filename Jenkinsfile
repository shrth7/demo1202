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
                 
            }
        }
        stage("Approval to run container"){
            steps{
                input "Build and Run Container?"
            }
        }
        stage('deploy') {
//             when{
//             branch "new_branch"
//             }
            steps{
            retry(3){
                
                sh 'docker run -d --name container${BUILD_NUMBER} imageagain5:5'
                sh 'docker container ls'
                }
            }
        }  
        
    }
    post{
            success{
                emailext body: '''Hello Sharath,

                As mentioned above the recent build in the pipeline new_pipe was successful
                ''', subject: 'Build Successful!!', to: 's.sharath2@in.bosch.com'
                
                sh 'docker kill $(docker ps -q)'
                sh 'docker rmi -f $(docker images -q)'
                sh 'docker rm $(docker ps -a -q)'
                sh 'docker container ls'
                sh 'docker images'
            }
            failure{
                emailext body: '''Hello Sharath,

                As mentioned above the recent build in the pipeline new_pipe has failed
                ''', subject: 'Build FAILED!!', to: 's.sharath2@in.bosch.com'
            }
            aborted{
                emailext body: '''Hello Sharath,

                As mentioned above the recent build in the pipeline new_pipe was aborted
                ''', subject: 'Build Aborted!!', to: 's.sharath2@in.bosch.com'
            }
        }
}
