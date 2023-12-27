pipeline {
    agent none
	triggers {
	        pollSCM '* * * *'
		    cron '* ** * *'
	}
	parameters {
			string (defaultValue: 'chaithra', description: 'Enter the username', name: 'USER_NAME')
			choice (name: 'ENVIRONMENT', choices: ['dev', 'qa', 'prod'], description: 'enter the place name')
			booleanParam 'my_boolean_para'
	}
	environment {
			env = "${params.ENVIRONMENT}"
            usr = "${params.USER_NAME}"
	}
	stages{
		stage("1st stage") {
			agent {label 'tomcat'}
			steps {
				echo "the job name = ${env.JOB_NAME}"
				echo "the job_url = ${JOB_URL}"
				echo "the user = ${usr}"
			}
		}	
		stage("2nd stage") {
			agent {label 'sonar'}
			steps {
				echo "the build number of this job = ${env.BUILD_NUMBER}"
				echo "this job is created by $USER_NAME"
			}
		}
		stage("3rd stage") {
			agent {label 'sonar'}
			when {
                allOf {
                    expression { currentBuild.resultIsBetterOrEqualTo('SUCCESS') }
                }
				branch 'development'
            }
			steps {
				echo "the jenkins_url = ${JENKINS_URL}"
			}
		}
		stage("4th stage") {
			agent {label 'tomcat'}
			when {
				expression { currentBuild.resultIsBetterOrEqualTo('SUCCESS') }			
			}
			steps {
				build '5th_question_2'
			}
		}
	}
	post {
		success {
			emailext body: '''$JOB_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS:
            Check console output at $JOB_URL to view the results.''', subject: 'pipeline_email', to: 'chaithrajenkins@gmail.com'
		}
	}
}

