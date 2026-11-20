pipeline {
    agent any

    stages {

        stage('Build') {
            steps {
                echo 'Building Flask Application'

                dir('python-flask-app') {
                    sh '''
                        python3 -m venv .venv
                        .venv/bin/pip install -r requirements.txt
                    '''
                }
            }
        }

        stage('Test') {
            steps {
                echo 'Running Unit Tests'

                dir('python-flask-app') {
                    sh '''
                        .venv/bin/python -m pytest -v test_app.py --junitxml=test-results.xml
                    '''
                }

                junit 'python-flask-app/test-results.xml'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying Flask Application'

                sh '''
                    mkdir -p deployment

                    cp python-flask-app/app.py deployment/
                    cp python-flask-app/requirements.txt deployment/

                    echo "Deployment successful"
                    ls -l deployment/
                '''
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'deployment/**, python-flask-app/test-results.xml',
                             allowEmptyArchive: true
        }

        success {
            echo 'Pipeline completed successfully!'
        }

        failure {
            echo 'Pipeline failed!'
        }
    }
}
