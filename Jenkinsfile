pipeline {
    agent { label 'OPENJDK-11-5' }
    parameters {
        choice(name: 'BRANCH_TO_BUILD', choices: ['main', 'task'], description: 'Branch to build')
        string(name: 'MAVEN_GOAL', defaultValue: 'package', description: 'maven goal')
    }
    stages {
        stage('vcs') {
            steps {
                git  url: 'https://github.com/Sufiyan779/spring-petclinic-1.git',
                     branch: "${params.BRANCH_TO_BUILD}"
            }
        }
        stage ('build packages') {
            steps {
            sh "/usr/share/maven/bin/mvn ${params.MAVEN_GOAL}"
            }
        }
        stage ('Artifactory configuration') {
            steps {
				rtMavenDeployer (
                    id: "MAVEN_DEPLOYER",
                    serverId:    "JFROG_ID",
                    releaseRepo:  "sufiyan-libs-release-local",
                    snapshotRepo: "sufiyan-libs-snapshot-local"
                )
            }
        }
        stage ('Exec Maven') {
            steps {
                rtMavenRun (
                    tool:  "MVN-3.6.3",
                    pom:   "pom.xml",
                    goals: "clean install",
                    deployerId: "MAVEN_DEPLOYER",
                )
            }
        }
        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "JFROG_ID"
                )
            }
        }
        stage ('archive results') {
            steps {
            junit '**/surefire-reports/*.xml'
            }
        }
    } 
}           