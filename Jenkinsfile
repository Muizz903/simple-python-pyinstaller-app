pipeline {
    agent none
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Build') {
            agent {
                docker {
                    image 'python:2-alpine'
                }
            }
            steps {
                sh 'python -m py_compile sources/add2vals.py sources/calc.py'
            }
        }
        stage('Test') {
            agent {
                docker {
                    image 'qnib/pytest'
                }
            }
            steps {
                sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            }
            post {
                always {
                    junit 'test-reports/results.xml'
                }
            }
        }
        stage('Deliver') {
            agent {
                docker {
                    image 'cdrx/pyinstaller-windows:python2'
                }
            }
            steps {
                sh 'wine pyinstaller --onefile sources/add2vals.py'  // Use 'wine' to execute PyInstaller
            }
            post {
                success {
                    archiveArtifacts 'dist/add2vals.exe' // Archive the Windows EXE
                }
            }
        }
    }
}
