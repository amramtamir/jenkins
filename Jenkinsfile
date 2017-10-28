
stage("Build"){
    node("linux") {
        sh 'echo hello world'
    }
}
stage("DEV"){
    node("linux") {
        sh 'echo deploying to dev'
    }
}
def userInput = true
def didTimeout = false
def version
stage("QA"){
    node("linux") {
        sh 'echo deploying to dev'
    }
try {
    timeout(time: 120, unit: 'SECONDS') { // change to a convenient timeout for you
        userInput = input(
        id: 'Proceed1', message: 'Was this successful?', parameters: [
        [$class: 'BooleanParameterDefinition', defaultValue: true, description: '', name: 'Please confirm you agree with this'],
        [$class: 'TextParameterDefinition', description: 'Version Number', name: 'version']
        ])
    }
    sh echo "${version}" > .version
} catch(err) { // timeout reached or input false
    def user = err.getCauses()[0].getUser()
    if('SYSTEM' == user.toString()) { // SYSTEM means timeout.
        didTimeout = true
    } else {
        userInput = false
        echo "Aborted by: [${user}]"
    }
}
}

stage("deploy"){

node ("linux"){
    if (didTimeout) {
        // do something on timeout
        echo "no input was received before timeout"
    } else if (userInput == true) {
        // do something
        version = readFile '.version'
        echo "this was successful, deploying : ${version}"
    } else if (userInput == false) {
        // do something else
        echo "this was not successful"
        // currentBuild.result = 'FAILURE'
    }
}
}