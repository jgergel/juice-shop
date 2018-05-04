pipeline {
	agent any
	stages {
		stage('Build'){
			steps {
				nodejs('nodjs_9_11_1_auto') {
					sh 'npm install'
				}
			}		
// following lines can be used instead of auto installing during the build
// in this case npm v9.11.1 is installed on build server located in the apps directory      
//			steps {
//            	sh 'export PATH=$PATH:/apps/node-v9_11_1/bin/; npm install'
//			}		
      	}
		stage('Policy Evaluation Dev'){
			steps {
				script {
// this line runs the nexusPolicyEvaluation Jenkins plugin action  and
// creates a environmet variable 'evaluation' storing the results of the nexusPolicyEvaluation execution 
        			def evaluation = nexusPolicyEvaluation failBuildOnNetworkError: false, iqApplication: 'JuiceShop', iqScanPatterns: [[scanPattern: 'node_modules/**/*']], iqStage: 'build', jobCredentialsId: ''
// create file storing the Nexus Policy Evaluation URL
					echo "${evaluation.applicationCompositionReportUrl}" >> ./lastNexusPolicyEvaluationURL
				}
			}
		}
		stage('Publish/Deploy to Dev Repo'){
			steps {
        		sh 'export PATH=$PATH:/apps/node-v9_11_1/bin/; npm publish'
			}
		}
	}
}
