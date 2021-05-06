pipeline {
	agent any
	
	tools {nodejs "node" }

	stages{
		stage('Build') {
			steps{
				script{
					LAST_STAGE_NAME = env.STAGE_NAME
				}
				echo 'Building...'
				sh 'npm installl'
			}
		}
		stage('Test') {
			when {
              			expression {currentBuild.result == null || currentBuild.result == 'SUCCESS'}
            		}
            		steps {
                		echo 'Testing..'
				sh 'npm test'
            		}
		}
		stage('Deploy') {
            		steps {
                		echo 'Deploying....'
            		}
        }
	}
	post{
		failure{
			emailext attachLog: true,
				body: "Jenkins ${last_stage_name} ended with status: ${currentBuild.currentResult} of job ${env.JOB_NAME}",
                		to: 'szymonxlorenc@gmail.com',
				subject: "${env.JOB_NAME} -> ${LAST_STAGE_NAME}: ${currentBuild.currentResult}"
		}
		success{
			emailext attachLog: true,
                		body: "Jenkins builded and tested with status: ${currentBuild.currentResult} of job ${env.JOB_NAME}",
                		to: 'szymonxlorenc@gmail.com',
				subject: "${env.JOB_NAME}: ${currentBuild.currentResult}"
		}		
	}
}
