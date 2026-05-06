pipeline{
	agent any
	environment{
		IMAGE_NAME = "demoNginx"
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
                    		echo "Checking workspace..."
                    		ls -la

                    		echo "Installing Trivy if not present..."
                    		if ! command -v trivy &> /dev/null
                   			 then
                        		wget https://github.com/aquasecurity/trivy/releases/latest/download/trivy_0.50.0_Linux-64bit.deb
                        		sudo dpkg -i trivy_0.50.0_Linux-64bit.deb
                    		fi

                    		echo "Running Trivy filesystem scan..."
                    		trivy fs . --severity HIGH,CRITICAL --exit-code 1
                		'''
                       }
                }
                stage('5. Build Docker Image'){
                        steps {
                          sh '''
                              docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .
                                
                          '''
                       }
                }
                stage('6. Build Image Scan'){
                        steps {
                          sh '''
                              trivy image ${IMAGE_NAME}:${IMAGE_TAG}

                          '''
                       }
                }
	}
}
