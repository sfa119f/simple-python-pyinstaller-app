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
    docker.image('cdrx/pyinstaller-linux:python2').inside {
        stage('Deploy') {
            try {
                sh 'pyinstaller --onefile sources/add2vals.py'
                archiveArtifacts 'dist/add2vals'
            } catch (err) {
                echo 'Something failed' + err.getMessage()
            }
        }
    }
}
