properties([[$class: 'BuildDiscarderProperty', strategy: [$class: 'LogRotator', artifactNumToKeepStr: '2', numToKeepStr: '2']]])

node {
    def projectName = "janus-impstar"
    
    def projectDir = pwd()+ "/${projectName}"
    def artifactDir = "${projectName}/build"

    try {
        sh "mkdir -p '${projectDir}'"
        dir (projectDir) {
            stage("Checkout") {
                checkout scm
            }
            stage("submodules") {
                sh 'git submodule update --init'
            }
            stage("make all") {
                sh "cd build_scripts && ./janus.bash '${artifactDir}'"
            }
            stage("deploy") {
                archiveArtifacts artifacts: "${artifactDir}/opt/janus/**/*", fingerprint: true
            }
            currentBuild.result = "SUCCESS"
        }
    } catch(e) {
        currentBuild.result = "FAILURE"
        throw e
    }
}