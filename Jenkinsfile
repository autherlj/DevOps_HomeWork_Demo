node {

    checkout scm

    env.DOCKER_API_VERSION="1.35"
    
    sh "git rev-parse --short HEAD > commit-id"

    tag = readFile('commit-id').replace("\n", "").replace("\r", "")
    appName = "devopshomework"
    registryHost = "autherlj/mytest:"
    imageName = "${registryHost}${appName}:${tag}"
    env.BUILDIMG=imageName

    stage "Build"
    
        sh "docker build -t ${imageName} -f Dockerfile ."
    
    stage "Push"

        sh "docker push ${imageName}"

    stage "Deploy"

        sh "sed 's#autherlj/devopshomework#'$BUILDIMG'#' php-mysql.yaml | kubectl apply -f -"
        sh "kubectl rollout status deployment/php-mysql"
}
