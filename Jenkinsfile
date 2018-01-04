#!/usr/bin/env groovy

pipeline {
    agent {
        label "${params.JENKINS_SLAVE_LABEL}"
    }
    options {
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
                withMaven(maven: "${params.MAVEN_VERSION}") {
                    sh 'mvn clean package assembly:single -Dmaven.test.skip=true'
                    sh 'cp -vf target/aws-maven-*.jar $M2_HOME/lib/'
                }
            }
        }
    }
}
