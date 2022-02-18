#!groovy

@Library('alayacare@v0.7.24') _

node('build') {
    def dockerImage
    String repository = env.JOB_NAME.split('/')[1]

    try {
        String buildTag = UUID.randomUUID().toString()

        stage('Git Checkout') {
            checkout scm
        }

        stage('Build Docker Image') {
            dockerImage = docker.build(buildTag, "./util/")
        }

        stage('Push Image') {
            ci.pushDockerImageToRegistry(dockerImage: dockerImage, repository: repository)
        }
    } catch(exception) {
        println(exception)
        throw exception
    } finally {
        if (dockerImage) {
            sh("docker rmi ${dockerImage.id} || true")
        }
        cleanWs()
    }
}
