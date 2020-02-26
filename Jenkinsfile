pipeline {
    agent none
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
                    //slackUploadFile channel: "#mcms-iscp-developers", filePath: 'test-reports/results.xml', initialComment: 'report file'
                }
            }
        }
        stage('Deliver') {
            agent {
                docker {
                    image 'cdrx/pyinstaller-linux:python2'
                }
            }
            steps {
                sh 'pyinstaller --onefile sources/add2vals.py'
                //slackUploadFile channel: "#mcms-iscp-developers", filePath: 'sources/add2vals.py', initialComment: 'python file'
            }
            post {
                success {
                    archiveArtifacts 'dist/add2vals'
                  //  slackUploadFile channel: "#mcms-iscp-developers", filePath: 'dist/add2vals', initialComment: 'python report file'
                    
                    
                }
            }
        }
    }
}
