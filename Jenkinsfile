pipeline {
    agent {
        docker {
            image 'python:3.9'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'python -m py_compile sources/add2vals.py sources/calc.py'
                stash(name: 'compiled-results', includes: 'sources/*.py*')
            }
        }
        stage('Test') {
            steps {
                sh 'sudo pip install pytest'
                sh 'pytest sources/test_calc.py'
                stash(name: 'test-results', includes: 'sources/test_calc.py')
            }
        }
        stage('Approval') {
            steps {
                input message: 'Lanjutkan ke tahap Deploy? (Klik "Proceed" untuk melanjutkan ke tahap Deploy)'
            }
        }
        stage('Deploy') {
            steps {
                sh 'sudo pip install pyinstaller'
                sh 'pyinstaller --onefile sources/add2vals.py'
                sleep time: 1, unit: 'MINUTES'
                stash(name: 'deploy-results', includes: 'dist/*')
            }
        }
    }
}
