pipeline {
    agent {label 'OPENJDK-11-3' }
	parameters {
	choice(name: 'BRANCH', choices: [ 'task''main'], description: 'select branch to build')
	string(name: 'GOAL', defaultValue: 'package', description: 'our Goal')
	}
	triggers{
	      pollSCM('* * * * *')
	}
	
    stages {
        stage ('source code  from git remote repository') {
            steps {
                git url: 'https://github.com/subrAt5238/spring-petclinic.git',
                branch: "${params.BRANCH}"
            }
        }
        stage('To build maven package') {
            steps {
                sh "mvn ${params.GOAL}"
            }
        }
		stage("archive artifact") {
            steps {
                archiveArtifacts artifacts: 'target/*.jar'
            }
			steps {
                junit '**/surefire-reports/*.xml'
            }
        }
				
    }
}