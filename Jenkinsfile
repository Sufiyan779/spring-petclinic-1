pipeline 
{
    agent {label 'OPENJDK-11-3'}
     parameters 
     {
        choice(name: 'BRACH_TO_BUILD', choices: ['main', 'task'], description: 'Choose the branch')
     }
	
    stages {
        stage ('source code  from git remote repository')
         {
            steps 
            {
               git url: 'https://github.com/Sufiyan779/spring-petclinic-1.git',
                    branch: "${params.BRANCH_TO_BUILD}"

            }
        }
        stage('To build maven package') 
        {
            steps 
            {
                sh 'mvn package'
            }        
        }
        stage('archive results') 
        {
            steps 
            {
                junit '**/surefire-reports/*.xml'
            }
        }
    }
}