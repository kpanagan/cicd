pipeline {
	agent any 
	environment {
		//Jenkins variables can be found http://localhost:8085/env-vars.html/
		BRANCH_NAME = 'main'
		NEW_VERSION = '1.3.0'
		SERVER_CREDENTIALS = credentials('git-repo-server-credentials')
	}
	stages {
		stage("build") {
			steps {
				echo 'building the application...'
				echo "building the version ${NEW_VERSION}"
				echo "building with ${SERVER_CREDENTIALS}"
			}
		}
		stage("test") {
			when {
				expression {
					BRANCH_NAME == 'main' || BRANCH_NAME == 'dev'
				}
			}
			steps {
				echo 'testing the application...'
			}
		}
		stage("deploy") {
			steps {
				echo 'deploying the application...'
				withCredentials([
					usernamePassword(credentials: 'git-repo-server-credentials', usernameVariable: USER, passwordVariable: PWD)
				]) {
					echo "User:  ${USER}"
					echo "Password:  ${PWD}"
				}
			}
		}
	}
	post {
		always {
			echo 'send email about Jenkins build status'
		}
		failure {
			echo 'Build status alert'
		}
	}
}