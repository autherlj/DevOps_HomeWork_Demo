node {

    checkout scm

    env.DOCKER_API_VERSION="1.35"
    
    sh "git rev-parse --short HEAD > commit-id"

    tag = readFile('commit-id').replace("\n", "").replace("\r", "")
    registryHost = "autherlj/mytest"
    imageName = "${registryHost}:${tag}"
    env.BUILDIMG=imageName

    stage "Build"
    
        sh "docker build -t ${imageName} -f Dockerfile ."
    
    stage "Push"

        sh "docker push ${imageName}"

    stage "Deploy"

        sh "sed 's#autherlj/mytest#'$BUILDIMG'#' php-mysql.yaml | kubectl apply -f -"
        sh "kubectl rollout status deployment/php-mysql -n uat"
}
