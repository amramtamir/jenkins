
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
def userInput = null
def didTimeout = false
def version
stage("QA"){
    node("linux") {
        sh 'echo deploying to dev'
    }
try {
    timeout(time: 120, unit: 'SECONDS') { // change to a convenient timeout for you
        userInput = input(
        id: 'Proceed1', message: 'Approve deployment? (add version number)', parameters: [
        [$class: 'BooleanParameterDefinition', defaultValue: true, description: '', name: 'confirm'],
        [$class: 'TextParameterDefinition', description: 'Version Number', name: 'version']
        ])
    }
} catch(err) { // timeout reached or input false
    def user = err.getCauses()[0].getUser()
    if('SYSTEM' == user.toString()) { // SYSTEM means timeout.
        didTimeout = true
    } else {
        userInput = null
        echo "Aborted by: [${user}]"
    }
}
}

stage("deploy"){

node ("linux"){
    if (didTimeout) {
        // do something on timeout
        echo "no input was received before timeout"
    } else if (userInput["confirm"] == true) {
        // do something

        echo "this was successful, deploying : "+userInput["version"]
    } else if (userInput == false) {
        // do something else
        echo "this was not successful"
        // currentBuild.result = 'FAILURE'
    }
}
}