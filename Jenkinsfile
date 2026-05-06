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
                    		trivy fs . --severity HIGH,CRITICAL || true
                		'''
                       }
                }
                stage('5. Build Docker Image'){
                        steps {
                          sh '''
                              docker version
							  docker build -t demoNginx:v1 .
							  
                                
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
	}
}
