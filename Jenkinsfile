node ('docker') {
    checkout scm
    def workspace = pwd()

    stage('Build image one') {
        // image one
        dir("${env.WORKSPACE}/one") {
        def customImageOne = docker.build("demouser/appimageone:${BUILD_NUMBER}")
        }
    }

    stage('Scan image with Aqua') {
        aqua locationType: 'local', localImage: 'demouser/appimageone:${BUILD_NUMBER}', hideBase: false,  notCompliesCmd: '', onDisallowed: 'fail', showNegligible: false
    }


    stage('Build image two') {
        // image two
        dir("${env.WORKSPACE}/two") {
        def customImageTwo = docker.build("demouser/appimagetwo:${BUILD_NUMBER}")
        }
    }


    stage('Scan image two with Aqua') {
        aqua locationType: 'local', localImage: 'demouser/appimagetwo:${BUILD_NUMBER}', hideBase: false,  notCompliesCmd: '', onDisallowed: 'fail', showNegligible: false
    }

    stage('Push to registry') {
        // Going to skip this part for testing.
    }
    
}
