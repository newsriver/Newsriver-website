#!groovy​


def marathonAppId = '/newsriver-website'
def projectName = 'newsriver-website'
def dockerRegistry = 'docker-registry.newsriver.io:5000'
def marathonURL = 'http://46.4.71.105:8080/'

node {

    stage 'checkout project'
    checkout scm

    deployDockerImage(projectName, dockerRegistry)
    restartDockerContainer(marathonAppId, projectName, dockerRegistry, marathonURL)

}


def restartDockerContainer(marathonAppId, projectName, dockerRegistry, marathonURL) {
    stage 'deploy application'
    marathon(
            url: "$marathonURL",
            forceUpdate: true,
            appid: "$marathonAppId",
            docker: "$dockerRegistry/$projectName:${env.BUILD_NUMBER}"
    )
}

def deployDockerImage(projectName, dockerRegistry) {

    stage 'build'
    initDocker()



    docker.withRegistry("https://$dockerRegistry/") {
        stage 'build docker image'
        def image = docker.build("$projectName:latest")
        stage 'upload docker image'
        image.push(env.BUILD_NUMBER)
    }


}


def initDocker() {
    def status = sh(script: 'docker ps', returnStatus: true)
    if (status != 0) {
        sh 'service docker start'
    }
}
