node {

    checkout scm

    env.DOCKER_API_VERSION="1.23"
    
    sh "git rev-parse --short HEAD > commit-id"

    tag = readFile('commit-id').replace("\n", "").replace("\r", "")
    appName = "hello-kenzan"
    registryHost = "192.168.108.53:5000/"
    imageName = "${registryHost}${appName}:${tag}"
    latestImageName = "${registryHost}${appName}:latest"
    env.BUILDIMG=imageName

    stage "Build"
    
        sh "docker build -t ${imageName} -f applications/hello-kenzan/Dockerfile applications/hello-kenzan"
    
    stage "Push"

        sh "docker push ${imageName}"

	// Tag latest and push again.
	sh "docker tag ${imageName} ${latestImageName}"
	sh "docker push ${latestImageName}"

    stage "Delete"

    	sh "kubectl delete -f applications/hello-kenzan/k8s/deployment.yaml"

    stage "Deploy"

        //kubernetesDeploy configs: "applications/${appName}/k8s/*.yaml", kubeconfigId: 'kenzan_kubeconfig'
	sh "kubectl apply -f applications/hello-kenzan/k8s/deployment.yaml"

}
