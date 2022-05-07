pipeline {
    agent any

    stages {
        stage('checkout') {
            steps {
                git branch: 'develop', credentialsId: 'a1380de1-1479-4edc-9828-0e5a24efa6e6', url: 'ssh://liuwen@10.0.3.14:29418/monitorserver.git'
            }
        }
        stage('package'){
            steps{
                bat "E:/maven/apache-maven-3.8.3/bin/mvn clean package"
            }
        }
        stage('backup'){
            steps{
                sshPublisher(publishers: [sshPublisherDesc(configName: '10.0.10.132', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '''cd /opt/monitor-server/interface-server
./bak.sh''', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
            }
        }
        stage('upload'){
            steps{
                bat "pscp -l root -pw DtcorUQf4kg= E:/jenkins/workspace/pipeline/interfaceserver/target/interfaceserver.jar root@10.0.10.132:/opt/monitor-server/interface-server < E:/jenkins/test.bat "
            }
        }
        stage('start'){
            steps{
                sshPublisher(publishers: [sshPublisherDesc(configName: '10.0.10.132', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '''cd /opt/monitor-server/interface-server
./restart.sh''', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '', usePty: true)], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: true)])
            }
        }
    }
}
