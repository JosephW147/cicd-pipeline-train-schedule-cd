pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('DeployToStaging') {
            when { 
                branch 'master'
            }
            steps {
                sshPublisher(
                    publishers: [
                        sshPublisherDesc(
                            configName: 'staging', 
                            sshCredentials: [
                                encryptedPassphrase: '{AQAAABAAAAAQEQDyeTfruRDYkqE1xS56GLbg7AbIfFzQiHLwO1l/CpI=}', 
                                key: '', 
                                keyPath: '', 
                                username: 'deploy'
                            ], 
                            transfers: [
                                sshTransfer(
                                    cleanRemote: false, 
                                    excludes: '', 
                                    execCommand: 'sudo systemctl stop train-schedule && rm -rf /opt/train-schedule/* && sudo unzip /tmp/trainSchedule.zip -d /opt/train-schedule/ && sudo systemctl start train-schedule', 
                                    execTimeout: 120000, 
                                    flatten: false, 
                                    makeEmptyDirs: false, 
                                    noDefaultExcludes: false, 
                                    patternSeparator: '[, ]+', 
                                    remoteDirectory: '/tmp', 
                                    remoteDirectorySDF: false, 
                                    removePrefix: 'dist/', 
                                    sourceFiles: 'dist/trainSchedule.zip'
                                )
                            ], 
                            usePromotionTimestamp: false, 
                            useWorkspaceInPromotion: false, 
                            verbose: true
                        )
                    ]
                )
             
                /*   withCredentials([usernamePassword(credentialsId: 'webserver_login', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
                    sshPublisher(
                        failOnError: true,
                        continueOnError: false,
                        publishers: [
                            sshPublisherDesc(
                                configName: 'staging',
                                sshCredentials: [
                                    username: "$USERNAME",
                                    encryptedPassphrase: "$USERPASS"
                                    ],
                                transfers: [
                                    sshTransfer(
                                        sourceFiles: 'dist/trainSchedule.zip',
                                        removePrefix: 'dist/',
                                        remoteDirectory: '/tmp',
                                       // execCommand: 'sudo /usr/bin/systemctl stop train-schedule',
                                      //  execCommand: 'rm -rf /opt/train-schedule/*',
                                        execCommand: 'unzip /tmp/trainSchedule.zip -d /opt/train-schedule'
                                      //  execCommand: 'sudo /usr/bin/systemctl start train-schedule'
                                        )
                                    ]
                                )
                            ]
                        )
                    } */
                }
            }
          /*  stage('DeployToProduction') {
                when {
                    branch 'master'
                }
                steps {
                    input 'Does the staging environment look OK?'
                    milestone(1)
                    withCredentials([usernamePassword(credentialsId: 'webserver_login', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
                    sshPublisher(
                        failOnError: true,
                        continueOnError: false,
                        publishers: [
                            sshPublisherDesc(
                                configName: 'production',
                                sshCredentials: [
                                    username: "$USERNAME",
                                    encryptedPassphrase: "$USERPASS"
                                ], 
                                transfers: [
                                    sshTransfer(
                                        sourceFiles: 'dist/trainSchedule.zip',
                                        removePrefix: 'dist/',
                                        remoteDirectory: '/tmp',
                                        execCommand: 'sudo /usr/bin/systemctl stop train-schedule && rm -rf /opt/train-schedule/* && unzip /tmp/trainSchedule.zip -d /opt/train-schedule && sudo /usr/bin/systemctl start train-schedule'
                                    )
                                ]
                            )
                        ]
                    )
                }
            }
        } */
    }
}
