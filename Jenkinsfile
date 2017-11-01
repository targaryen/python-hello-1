node {
    checkout scm

    def customImage = docker.build("image:${env.BUILD_ID}")

    stage('Run Aqua Scan'){
        def buildResult = 'success'
        echo 'Running Aqua Scan'
        try{
            sh '/usr/bin/docker run --rm -v /var/run/docker.sock:/var/run/docker.sock -v '+env.WORKSPACE+':/reports registry.devopslab.co:5000/aquasec/scanner-cli:2.6.0 --local -image image_to_scan:tag --host ${AQUA_SERVER_URL} --user ${AQUA_USER} --password ${AQUA_PASS} --htmlfile /reports/aqua-scan.html'
        }catch(e){
            buildResult = 'failure'
            currentBuild.result = buildResult
            error("Build failed due to high vulnerability on image")
       } finally {
                publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: true, reportDir: './', reportFiles: 'aqua-scan.html', reportName: 'Aqua Scan Results'])
        }
    }

}