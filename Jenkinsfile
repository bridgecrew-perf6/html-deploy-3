pipeline {

  agent any
  environment {
    DOCKER_IMAGE = "truongtuan1291/html-deploy"
  }

  stages {
      
    stage("build") {
            
        steps {
        
        withDockerRegistry(credentialsId: 'user-dockerhub', url: 'https://index.docker.io/v1/') {
            
            sh "docker build -t ${DOCKER_IMAGE} . "
            sh "docker push ${DOCKER_IMAGE}"
        }    

            //clean to save disk
            sh "docker image rm -f ${DOCKER_IMAGE}"
            sh "docker image prune -f"
        }

    }
	  
    stage("ssh"){
            
        steps {
                
        sshPublisher(publishers: [sshPublisherDesc(configName: 'user-server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: """cd ~
            docker stop html-deploy-container
            docker rm -f html-deploy-container
            docker image rm -f truongtuan1291/html-deploy
            docker run -p 80:80 --name html-deploy-container -d truongtuan1291/html-deploy
            docker image prune -f""", execTimeout: 120000000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
        }
    } 

  }

}