node {
    stage('Build') {
        docker.image('python:2-alpine').inside {
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
        }
    }
    stage('Test') {
        docker.image('qnib/pytest').inside {
            try {
                sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            } catch (Exception e) {
                echo 'Error: ' + e.toString()
            } finally {
                always {
                    junit 'test-reports/results.xml'
                }
            }
        }
    }
    stage('Manual Approval') {
         // some block
        input message: 'Lanjutkan ke tahap Deploy?'
    }
    stage('Deploy') {
        docker.image('cdrx/pyinstaller-linux:python2').inside("--entrypoint=''") {
            try {
                sh 'pyinstaller --onefile sources/add2vals.py'
            } catch (Exception e) {
                echo 'Error: ' + e.toString()
            } finally {
                success {
                    archiveArtifacts 'dist/add2vals'
                    sleep 60
                }
            }
        }
    }
}