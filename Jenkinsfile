pipeline {
    agent any
   
    stages {

        stage('Run Selenium Tests with pytest') {
            steps {
                    echo "Running Selenium Tests using pytest"

                    // Install Python dependencies
                    bat 'pip install -r requirements.txt'

                    // ✅ Start Flask app in background
                    bat 'start /B python app.py'

                    // ⏱ Wait a few seconds for the server to start
                    bat 'ping 127.0.0.1 -n 5 > nul'

                    // ✅ Run tests using pytest
                    //bat 'pytest tests\\test_registrationapp.py --maxfail=1 --disable-warnings --tb=short'
                    bat 'pytest -v'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Build Docker Image"
                bat "docker build -t seleniumdemoapp:v1 ."
            }
        }
        stage('Docker Login') {
            steps {
                  bat 'docker login -u srivarshithameda -p #Varshithameda2344'
                }
            }
        stage('push Docker Image to Docker Hub') {
            steps {
                echo "push Docker Image to Docker Hub"
                bat "docker tag seleniumdemoapp:v1 srivarshithameda/sample:seleniumtestimage"               
                    
                bat "docker push srivarshithameda/sample:seleniumtestimage"
                
            }
        }
        stage('Deploy to Kubernetes') { 
            steps { 
                    // apply deployment & service 
                    bat 'kubectl apply -f deployment.yaml --validate=false' 
                    bat 'kubectl apply -f service.yaml' 
            } 
        }
    }
    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Please check the logs.'
        }
    }
}