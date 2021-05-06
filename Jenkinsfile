pipeline {
	agent any
	
	tools {nodejs "node" }

	environment {
       		LAST_STAGE_NAME = 'EMPTY'
   	}
	
	stages{
		stage('Build') {
			steps{
				script{
				env.LAST_STAGE_NAME = env.STAGE_NAME
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
				script{
				env.LAST_STAGE_NAME = env.STAGE_NAME
				}
                		echo 'Testing..'
				sh 'npm test'
            		}
		}
		stage('Deploy') {
            		steps {
				script{
				env.LAST_STAGE_NAME = env.STAGE_NAME
				}
                		echo 'Deploying....'
            		}
        	}
	}
	post{
		failure{
			emailext attachLog: true,
				body: "Jenkins ${env.LAST_STAGE_NAME} ended with status: ${currentBuild.currentResult} of job ${env.JOB_NAME}",
                		to: 'szymonxlorenc@gmail.com',
				subject: "${env.JOB_NAME} -> ${env.LAST_STAGE_NAME}: ${currentBuild.currentResult}"
		}
		success{
			emailext attachLog: true,
                		body: "Jenkins builded and tested with status: ${currentBuild.currentResult} of job ${env.JOB_NAME}",
                		to: 'szymonxlorenc@gmail.com',
				subject: "${env.JOB_NAME}: ${currentBuild.currentResult}"
		}		
	}
}
