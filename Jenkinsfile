pipeline {
    agent any
    tools {
        maven 'maven3.8.2'
    }
    stages {
        stage('github clone') {
            steps {
                git 'https://github.com/cjk2023/jenkins_pipeline_script'
            }
        }
        stage('build') {
            steps {
                sh '''
                    echo build start
                    mvn clean compile package -DskipTests=true
                '''
            }
        }
        stage('deploy') {
            steps {
              deploy adapters: [tomcat9(credentialsId: 'deployer_user', path: '', url: 'http://[ip]:8080/')], contextPath: null, war: '**/*.war'
            }
        }
        stage('ssh publisher') {
            steps {
               sshPublisher(publishers: [sshPublisherDesc(configName: 'ncp-docker-server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'docker build -t magiceco/devops_exam1 -f Dockerfile .', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '.', remoteDirectorySDF: false, removePrefix: 'target', sourceFiles: 'target/devops_demo-0.1.war')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
            }
        }
    }
}
