pipeline {
	agent any
	stages {
		stage('Build'){
			steps {
				nodejs('nodjs_9_11_1_auto') {
					sh 'npm install'
				}
			}		
// following lines commented out can be used instead of auto installing during the build
// in this case npm v9.11.1 is installed on build server located in the apps directory      
//			steps {
//            	sh 'export PATH=$PATH:/apps/node-v9_11_1/bin/; npm install'
//			}		
      	}
		stage('Policy Evaluation Dev'){
			steps {
				sh "pwd; whoami"
				script {
// this stript executes the nexusPolicyEvaluation Jenkins plugin action and
// creates a environmet variable 'evaluation' storing the results of the nexusPolicyEvaluation
// the Policy scan will only read the node_modules directory and its subdirectories, assumes node_modules is in the working directory
        			def evaluation = nexusPolicyEvaluation failBuildOnNetworkError: false, iqApplication: 'JuiceShop', iqScanPatterns: [[scanPattern: 'node_modules/**/*']], iqStage: 'build', jobCredentialsId: ''
// creates a file storing the Nexus Policy Evaluation URL
					echo "${evaluation.applicationCompositionReportUrl}"
					echo "${workspace}"
					def fileName = "LastNexusPolicyEvaluationURL"
					def outputFile = new File("${workspace}"+fileName)
					outputFile.write("${evaluation.applicationCompositionReportUrl}")
					echo "${outputFile}"
				}
			}
		}
		stage('Publish/Deploy to Dev Repo'){
			steps {
        		sh 'npm publish'
			}
//			steps {
//        		sh 'export PATH=$PATH:/apps/node-v9_11_1/bin/; npm publish'
//			}			
		}
	}
}
