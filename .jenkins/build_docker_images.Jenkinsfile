// The below timeout is set in minutes
GLOBAL_TIMEOUT = 240

DCAPTOOLS_REPO = "https://dcapdockerciregistry.azurecr.io"
DCAPTOOLS_REPO_CREDENTIAL_ID = "dcapdockerciregistry"

def buildDockerImages() {
    node("nonSGX") {
        timeout(GLOBAL_TIMEOUT) {
            stage("Checkout") {
                cleanWs()
                checkout scm
            }
			
            def dcap = load pwd() + "/.jenkins/src/Dcap.groovy"
			
            stage("Build Ubuntu 16.04 Docker Image") {
                docker.withRegistry(DCAPTOOLS_REPO, DCAPTOOLS_REPO_CREDENTIAL_ID) {
                    azDcapTools1604 = dcap.dockerImage("az-dcap-tools-16.04:${DOCKER_TAG}",
                                                     ".jenkins/Dockerfile",
                                                     "--build-arg UNAME=\$(id -un) --build-arg ubuntu_version=16.04")
                }
                pubazDcapTools1604 = dcap.dockerImage("oeciteam/az-dcap-tools-16.04:${DOCKER_TAG}",
                                                    ".jenkins/Dockerfile",
                                                    "--build-arg UNAME=\$(id -un) --build-arg ubuntu_version=16.04")
            }
            stage("Build Ubuntu 18.04 Docker Image") {
                docker.withRegistry(DCAPTOOLS_REPO, DCAPTOOLS_REPO_CREDENTIAL_ID) {
                    azDcapTools1804 = dcap.dockerImage("az-dcap-tools-18.04:${DOCKER_TAG}",
                                                     ".jenkins/Dockerfile",
                                                     "--build-arg UNAME=\$(id -un) --build-arg ubuntu_version=18.04")
                }
                pubazDcapTools1804 = dcap.dockerImage("oeciteam/az-dcap-tools-18.04:${DOCKER_TAG}",
                                                    ".jenkins/Dockerfile",
                                                    "--build-arg UNAME=\$(id -un) --build-arg ubuntu_version=18.04")
            }
            stage("Push to DCAP Docker Registry") {
                docker.withRegistry(DCAPTOOLS_REPO, DCAPTOOLS_REPO_CREDENTIAL_ID) {
                    azDcapTools1604.push()
                    azDcapTools1804.push()
                    if(params.TAG_LATEST == true) {
                        azDcapTools1604.push('latest')
                        azDcapTools1804.push('latest')
                    }
                }
            }
        }
    }
}

buildDockerImages()
