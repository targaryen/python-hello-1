node ('docker') {
    checkout scm

    docker.withRegistry(env.DOCKER_REGISTRY, 'labregistry') {

        def customImage = docker.build("demouser/appimage:${BUILD_NUMBER}")
            stage('Run Aqua Scan'){
                def buildResult = 'success'
                echo 'Running Aqua Scan'
                try{
                    sh '/usr/bin/docker run --rm -v /var/run/docker.sock:/var/run/docker.sock -v '+env.WORKSPACE+':/reports ${AQUA_SCANNER_IMAGE} --local --image demouser/appimage:${BUILD_NUMBER} --host ${AQUA_HOST} --user ${AQUA_USER} --password ${AQUA_PASSWORD} --htmlfile /reports/aqua-scan.html'
                }catch(e){
                    buildResult = 'failure'
                    currentBuild.result = buildResult
                    error("Build failed due to disallow result on Image Assurance from Aqua")
               } finally {
                        publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: true, reportDir: './', reportFiles: 'aqua-scan.html', reportName: 'Aqua Scan Results'])
                }
            }
        }




}