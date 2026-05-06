pipeline{
	agent any
	environment{
		IMAGE_NAME = "demonginx"
		IMAGE_TAG = "v1"
	}

	stages{

		stage('2. Clean Workspace'){
			steps {
		          cleanWs()
		       }
		}
		stage('3. Checkout'){
                        steps {
                          git branch: 'main', url: 'https://github.com/masterdevopsin/Nginx-Dummy.git'
                       }
                }
                stage('4. File System Scan'){
                        steps {
                          sh '''
                    		trivy fs . --severity HIGH,CRITICAL || true
                		'''
                       }
                }
                stage('5. Build Docker Image'){
                        steps {
                          sh '''
						  	  echo "Checking Docker..."
                              docker --version

							  echo "Building image..."
							  docker build -t demonginx:v1 .
							  
                                
                          '''
                       }
                }
                stage('6. Build Image Scan'){
                        steps {
                          sh '''
                              trivy image demoNginx:v1 --severity HIGH,CRITICAL || true

                          '''
                       }
                }
				stage('7. Deploy to Kubernetes Cluster') {
			    steps {
			        sh '''
			            echo "Checking cluster connection..."
			            kubectl get nodes
			
			            echo "Deploying application..."
			
			            kubectl apply -f deployment.yaml
			            kubectl apply -f service.yaml
			
			            echo "Updating image..."
			            kubectl set image deployment/demo-nginx \
			                demo-nginx=demonginx:v1
			
			            echo "Waiting for rollout..."
			            kubectl rollout status deployment/demo-nginx
			        '''
			    }
			}
	}
}
