pipeline {
	agent any
	
	tools {nodejs "node" }

	environment { 
        	LAST_STAGE_NAME = 'EMPTY'
    	}
	
	stages{
		stage('Build'){
			steps{
				script{
					echo 'Building...'
					LAST_STAGE_NAME = env.STAGE_NAME
					sh 'npm install'
				}
				
			}
		}
		stage('Test'){
			steps{
				script{
					echo 'Testing...'
					LAST_STAGE_NAME = env.STAGE_NAME
					sh 'npm run test'
				}
			}
		}
	}
	post{
		failure{
			emailext attachLog: true,
				body: "Jenkins ${last_stage_name} ended with status: ${currentBuild.currentResult} of job ${env.JOB_NAME}",
                		to: 'szymonxlorenc@gmail.com',
				subject: "${env.JOB_NAME} -> ${last_stage_name}: ${currentBuild.currentResult}"
		}
		success{
			emailext attachLog: true,
                		body: "Jenkins builded and tested with status: ${currentBuild.currentResult} of job ${env.JOB_NAME}",
                		to: 'szymonxlorenc@gmail.com',
				subject: "${env.JOB_NAME}: ${currentBuild.currentResult}"
		}		
	}
}
