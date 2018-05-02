#!/usr/bin/env groovy

def MAVEN_BUILD = 'mvn clean package assembly:single -Dmaven.test.skip=true'
def COPY_JAR_UNIX = 'cp -vf target/aws-maven-*.jar $M2_HOME/lib/'
def COPY_JAR_WINDOWS = 'copy /y target\\aws-maven-*.jar %M2_HOME%\\lib\\'

def createJob(nodeName) {
    return {
        node(nodeName) {
            checkout scm
            tool type: 'maven', name: params.MAVEN_VERSION
            if (isUnix()) {
                sh MAVEN_BUILD
                sh COPY_JAR_UNIX
            } else {
                bat MAVEN_BUILD
                bat COPY_JAR_WINDOWS
            }
        }
    }
}

pipeline {
    agent none
    options {
        skipDefaultCheckout()
        ansiColor('xterm')
    }
    parameters {
        string(name: 'JENKINS_SLAVE_LABEL',
            defaultValue: 'none',
            description: 'Specify the label of jenkins slave to run.')
        string(name: 'MAVEN_VERSION',
            defaultValue: '3.3.9',
            description: 'Maven version to use.')
    }
    stages {
        stage('Maven Package') {
            steps {
                script {
                    assert params.JENKINS_SLAVE_LABEL
                    assert params.MAVEN_VERSION
                    def jobs = [:]
                    def nodeNames = nodesByLabel params.JENKINS_SLAVE_LABEL
                    for (name in nodeNames) {
                        jobs[name] = createJob(name)
                    }
                    parallel jobs
                }
            }
        }
    }
}
