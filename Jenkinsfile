node {
    docker.image('python:2-alpine').inside {
        stage('Build') {
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
        }
    }
    docker.image('qnib/pytest').inside {
        stage('Test') { 
            try {
                sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            } finally {
                junit 'test-reports/results.xml'
            }
        }
    }
    // docker.image('cdrx/pyinstaller-linux:python2').inside('-it') {
    //     stage('Deploy') {
    //         try {
    //             input message: '(Click "Proceed" to continue)'
    //             echo '---[START DEPLOY]---'
    //             sh 'pyinstaller --onefile sources/add2vals.py'
    //             echo '---[FINISH DEPLOY]---'
    //             archiveArtifacts 'dist/add2vals'
    //         } catch (err) {
    //             echo 'Something failed' + err.getMessage()
    //         }
    //     }
    // }
}
