pipeline {
    agent any

    options {
        skipStagesAfterUnstable()
    }

    environment {
        PYTHON = 'C:\\Users\\chava\\AppData\\Local\\Programs\\Python\\Python312\\python.exe'
    }

    stages {

        stage('Build') {
            steps {
                bat "\"%PYTHON%\" -m py_compile sources\\add2vals.py sources\\calc.py"
            }
        }

        stage('Test') {
            steps {
                bat "\"%PYTHON%\" -m pytest --verbose --junit-xml=test-reports\\results.xml sources\\test_calc.py"
            }
            post {
                always {
                    junit 'test-reports\\results.xml'
                }
            }
        }

        stage('Deliver') {
            steps {
                bat "\"%PYTHON%\" -m PyInstaller --onefile sources\\add2vals.py"
            }
            post {
                success {
                    archiveArtifacts artifacts: 'dist\\add2vals.exe', fingerprint: true
                }
            }
        }
    }
}
