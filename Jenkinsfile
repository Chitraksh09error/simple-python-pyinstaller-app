pipeline {
    agent any

    options {
        skipStagesAfterUnstable()
    }

    stages {

        stage('Build') {
            steps {
                // Compile Python files
                bat 'py -m py_compile sources\\add2vals.py sources\\calc.py'
            }
        }

        stage('Test') {
            steps {
                // Run unit tests
                bat 'py -m pytest --verbose --junit-xml=test-reports\\results.xml sources\\test_calc.py'
            }
            post {
                always {
                    // Publish test results
                    junit 'test-reports\\results.xml'
                }
            }
        }

        stage('Deliver') {
            steps {
                // Create Windows executable
                bat 'py -m PyInstaller --onefile sources\\add2vals.py'
            }
            post {
                success {
                    // Archive the generated .exe
                    archiveArtifacts artifacts: 'dist\\add2vals.exe', fingerprint: true
                }
            }
        }
    }
}
