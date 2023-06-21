node {
    stage('Build') {
        docker.image('python:2-alpine').inside {
            try {
                sh 'python -m py_compile sources/add2vals.py sources/calc.py'
                stash(name: 'compiled-results', includes: 'sources/*.py*')
            } catch (err) {
                echo "Caught: ${err}"
                currentBuild.result = 'FAILURE'
            }
        }
    }

    stage('Test') { 
        docker.image('qnib/pytest').inside {
            try {
                sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            } catch (err) {
                echo "Caught: ${err}"
                currentBuild.result = 'FAILURE'
            } finally {
                junit 'test-reports/results.xml'
            }
        }
    }

    stage('Deploy') {
        input message: 'Lanjutkan ke tahap Deploy?'
        withEnv(["VOLUME=${'$(pwd)/sources:/src'}", "IMAGE=${'cdrx/pyinstaller-linux:python2'}"]) {
            try {
                dir(path: env.BUILD_ID) { 
                    unstash(name: 'compiled-results')
                    sh "docker run --rm -v ${VOLUME} ${IMAGE} 'pyinstaller -F add2vals.py'" 
                }
                sleep(time: 1, unit: 'MINUTES')
                archiveArtifacts "${env.BUILD_ID}/sources/dist/add2vals" 
                sh "docker run --rm -v ${VOLUME} ${IMAGE} 'rm -rf build dist'"
            } catch (err) {
                echo "Caught: ${err}"
                currentBuild.result = 'FAILURE'
            }
        }
    }
}