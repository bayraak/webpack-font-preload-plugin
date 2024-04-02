node {
    def app
    stage('Clone repository') {
        checkout([$class: 'GitSCM',
                  branches: [[name: '*/master']],
                  doGenerateSubmoduleConfigurations: false,
                  extensions: [[$class: 'CloneOption', depth: 1],
                               [$class: 'GitHubAppCredentials', appId: '867979', installationId: 'jenkins-fax']],
                  submoduleCfg: [],
                  userRemoteConfigs: [[url: 'https://github.com/bayraak/webpack-font-preload-plugin.git']]])
    }
    stage('Build image') {
       app = docker.build("bayraak/kiii-jenkins")
    }
    stage('Push image') {
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            app.push("${env.BRANCH_NAME}-${env.BUILD_NUMBER}")
            app.push("${env.BRANCH_NAME}-latest")
            // signal the orchestrator that there is a new version
        }
    }
}
