node{
              currentBuild.result = "SUCCESS"  
    try{
           stage ('CheckoutCode'){
                checkout scm
                env.DOCKER_API_VERSION="1.35"
                sh "git rev-parse --short HEAD > commit-id"
                tag = readFile('commit-id').replace("\n", "").replace("\r", "")
                registryHost = "autherlj/mytest"
                imageName = "${registryHost}:${tag}"
                env.BUILDIMG=imageName       
           }
           stage ('BuildDockerImage'){
                sh "docker build -t ${imageName} -f Dockerfile ." 
           }
           stage ('Push'){
                sh "docker push ${imageName}"
           }
           stage('DeploytoK8s'){
                sh "sed 's#autherlj/mytest#'$BUILDIMG'#' php-mysql.yaml | kubectl apply -f -"
                
           }
      
       }catch(error)
       {
              currentBuild.result = "FAILURE"
              throw error       
       }  
}
