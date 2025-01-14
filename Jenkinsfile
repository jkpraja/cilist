pipeline {
    agent any

    environment {
        GIT_COMMIT_SHORT = sh (returnStdout: true, script: '''echo $GIT_COMMIT | head -c 7''')
    }

    stages {
        stage('Prepare .env') {
            steps {
                sh 'echo GIT_COMMIT_SHORT=$(echo $GIT_COMMIT_SHORT) > .env'
            }
        }

        stage('Build database') {
            steps {
                dir('database') {
                    sh 'docker build . -t cilist-pipeline-db:$GIT_COMMIT_SHORT'
                    sh 'docker tag cilist-pipeline-db:$GIT_COMMIT_SHORT jkpraja/cilist-pipeline-db:$GIT_COMMIT_SHORT'
                    sh 'docker tag cilist-pipeline-db:$GIT_COMMIT_SHORT jkpraja/cilist-pipeline-db:latest'
                    sh 'docker push jkpraja/cilist-pipeline-db:$GIT_COMMIT_SHORT'
                    sh 'docker push jkpraja/cilist-pipeline-db:latest'
                }
            }
        }

        stage('Build backend') {
            steps {
                dir('backend') {
                    sh 'docker build . -t cilist-pipeline-be:$GIT_COMMIT_SHORT'
                    sh 'docker tag cilist-pipeline-be:$GIT_COMMIT_SHORT jkpraja/cilist-pipeline-be:$GIT_COMMIT_SHORT'
                    sh 'docker tag cilist-pipeline-be:$GIT_COMMIT_SHORT jkpraja/cilist-pipeline-be:latest'
                    sh 'docker push jkpraja/cilist-pipeline-be:$GIT_COMMIT_SHORT'
                    sh 'docker push jkpraja/cilist-pipeline-be:latest'
                }
            }
        }

        stage('Build frontend') {
            steps {
                dir('frontend') {
                    sh 'docker build . -t cilist-pipeline-fe:$GIT_COMMIT_SHORT'
                    sh 'docker tag cilist-pipeline-fe:$GIT_COMMIT_SHORT jkpraja/cilist-pipeline-fe:$GIT_COMMIT_SHORT'
                    sh 'docker tag cilist-pipeline-fe:$GIT_COMMIT_SHORT jkpraja/cilist-pipeline-fe:latest'
                    sh 'docker push jkpraja/cilist-pipeline-fe:$GIT_COMMIT_SHORT'
                    sh 'docker push jkpraja/cilist-pipeline-fe:latest'
                }
            }
        }

        stage('Deploy to remote server') {
            steps {
                sshPublisher(publishers: [sshPublisherDesc(configName: 'remote-server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'docker compose -f pipeline-compose.yaml up -d', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '.env, pipeline-compose.yaml')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
            }
        }

    }
}