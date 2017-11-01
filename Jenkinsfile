node {
    checkout scm

    def customImage = docker.build("demouser/appimage:${BUILD_NUMBER}")

    stage('Run Aqua Scan'){
        def buildResult = 'success'
        echo 'Running Aqua Scan'
        try{
            sh '/usr/bin/docker run --rm -v /var/run/docker.sock:/var/run/docker.sock -v '+env.WORKSPACE+':/reports registry.devopslab.co:5000/aquasec/scanner-cli:2.6.0 --local --image demouser/appimage:${BUILD_NUMBER} --host http://env1 --user scanner --password scanner123 --htmlfile /reports/aqua-scan.html'
        }catch(e){
            buildResult = 'failure'
            currentBuild.result = buildResult
            error("Build failed due to high vulnerability on image")
       } finally {
                publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: true, reportDir: './', reportFiles: 'aqua-scan.html', reportName: 'Aqua Scan Results'])
        }
    }

}