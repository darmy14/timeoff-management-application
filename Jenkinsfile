//Pipeline Variables
def IMAGE_REPO='timeoff-gorilla'   
def IMAGE_TAG='${BUILD_NUMBER}' 
def REGION='us-east-1'  
def BRANCH_NODEJS='main'
def REPOSITORY='587764720640.dkr.ecr.us-east-1.amazonaws.com/${IMAGE_REPO}'
def NODEJS_REPO='git@github.com:darmy14/timeoff-management-application.git' 

pipeline{
    agent any
  
  stages {
  
    stage("Cloning Git"){
    
      steps{
               sh "mkdir nodejs-build"
                dir("nodejs-build"){
                     git(
                            [
                                 url: "ssh://${NODEJS_REPO}", 
                                 branch: "${BRANCH_NODEJS}", 
                            ]
                        )
               }
  
            }
    }
    stage("Build image"){
    
      steps{
           script {
                    if("${ROLLBACK}" == "No"){
                      
                    sh "cd nodejs-build/ && docker build -t $IMAGE_REPO ."
                }
                    else {
                    echo 'Skipping stage'
                    }
                }

      }
    }
    stage("Push image"){
    
      steps{
            script {
                    if("${ROLLBACK}" == "No"){
                        sh "aws ecr get-login-password --region ${REGION} | docker login --username AWS --password-stdin 587764720640.dkr.ecr.us-east-1.amazonaws.com"
                        sh "docker tag ${IMAGE_REPO}:latest ${REPOSITORY}${IMAGE_REPO}:qa-1.0.${BUILD_NUMBER}"
                        sh "docker push ${REPOSITORY}${IMAGE_REPO}:qa-1.0.${BUILD_NUMBER}"
                    }
                    else{
                        echo 'Skipping stage'
                    }
                    }
      }
    }
    
  }
}
