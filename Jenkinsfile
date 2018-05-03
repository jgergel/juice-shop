pipeline {
	agent any
	stages {
		stage('Build'){
//			steps {
//				nodejs('nodjs_9_11_1_auto') {
//					sh 'npm install'
//				}
//			}		
//      
			steps {
            	sh 'export PATH=$PATH:/apps/node-v9_11_1/bin/; npm install'
            }		
      	}
		stage('Policy Evaluation Dev'){
			steps {
        		nexusPolicyEvaluation failBuildOnNetworkError: false, iqApplication: 'JuiceShop', iqScanPatterns: [[scanPattern: 'node_modules/**/*']], iqStage: 'build', jobCredentialsId: ''
			}
		}
		stage('Publish/Deploy to Dev Repo'){
			steps {
        		sh 'export PATH=$PATH:/apps/node-v9_11_1/bin/; npm publish'
			}
		}
	}
}
