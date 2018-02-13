node ('docker') {
    checkout scm

    stage('Build image') {
        def customImage = docker.build("demouser/appimage:${BUILD_NUMBER}")

    }
    stage('Scan image with Aqua') {
        aqua locationType: 'local', localImage: 'alpine', hideBase: false,  notCompliesCmd: '', onDisallowed: 'fail', showNegligible: false
    }


    /*
    docker.withRegistry(env.DOCKER_REGISTRY, 'labregistry') {

        def customImage = docker.build("demouser/appimage:${BUILD_NUMBER}")
        stage('Scan image with Aqua') {
            def buildResult = 'success'
            echo 'Running Aqua Scan'
            try {
                withCredentials([usernamePassword(credentialsId: 'aquascanner', passwordVariable: 'SCANNERPASS', usernameVariable: 'SCANNERUSER')]) {
                    sh '/usr/bin/docker run --rm -v /var/run/docker.sock:/var/run/docker.sock -v '+env.WORKSPACE+':/reports ${AQUA_SCANNER_IMAGE} --local --image demouser/appimage:${BUILD_NUMBER} --host ${AQUA_HOST} --user $SCANNERUSER --password $SCANNERPASS --htmlfile /reports/aqua-scan.html'
                }
            } catch(e) {
                buildResult = 'failure'
                currentBuild.result = buildResult
                error("Build failed due to disallow result on Image Assurance from Aqua")
            } finally {
                    publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: true, reportDir: './', reportFiles: 'aqua-scan.html', reportName: 'Aqua Scan Results'])
            }
       }


       stage("Push image to registry") {
            customImage.push()
            withCredentials([usernameColonPassword(credentialsId: 'aquascanner', variable: 'USERPASS')]) {
                sh 'curl -X POST -u $USERPASS ${AQUA_HOST}/api/v1/scanner/registry/$AQUA_REGISTRY/image/demouser/appimage:${BUILD_NUMBER}/scan'
            }
       }
    }
    */
}
