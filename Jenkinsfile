node {
    agent {
        docker {
            image 'python:2-alpine'
        }
    }
    stage('Build') {
        sh 'python -m py_compile sources/add2vals.py sources/calc.py'
    }
    agent {
        docker {
            image 'qnib/pytest'
        }
    }
    stage('Test') {
        sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
    }
    post {
        always {
            junit 'test-reports/results.xml'
        }
    }
}
