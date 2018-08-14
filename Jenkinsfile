node {

    checkout scm

    env.DOCKER_API_VERSION="1.23"
    
    sh "git rev-parse --short HEAD > commit-id"

    tag = readFile('commit-id').replace("\n", "").replace("\r", "")
    appName = "hello-samplekuber"
    registryHost = "127.0.0.1:30400/"
    imageName = "${registryHost}${appName}:${tag}"
    env.BUILDIMG=imageName

    stage ('Build')
    {
        sh "docker build -t ${imageName} -f SampleforKuber/SampleforKuber/publish/dockerfile SampleforKuber/SampleforKuber/publish"
    }
    stage ('Push')
    {
        sh "docker push ${imageName}"
    }
    stage ('Deploy')
    {
	sh "kubectl  run hello-samplekuber --image=#'$BUILDIMG'# port 80"
        sh "kubectl rollout status deployment/hello-samplekuber"
	sh "kubectl expose hello-samplekuber --type='NodePort'"
     }

}