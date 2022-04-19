
pipeline {
    agent any
    parameters {
        string (
            name: 'ARTIFACT_NAME',
            defaultValue: 'pipeline.zip',
            description: 'some desciption I guess'
            )
    }
    stages {
        stage('Download') {
            steps {
                cleanWs()
                echo("Download stage!")
                dir('pipeline') {
                    git (
                        branch: 'pipeline',
                        url: 'https://github.com/rtrk-jenkins-course/jenkins-course'
                    )
                }
                rtDownload (
                    serverId: 'local-artifactory',
                    spec: ''' {
                        "files": [
                          {
                            "pattern": "example-repo-local/printer.zip",
                            "target": "pipeline-folder/"
                          }
                        ]
                    }'''
                    )
                unzip (
                    zipFile: "pipeline-folder/printer.zip",
                    dir: "pipeline/"
                    )

            }
        }
        stage('Build') {
            steps {
                echo("Build stage!")
                dir('pipeline') {
                    bat (
                        script: """
                            call Makefile.cmd
                        """
                        )
                }
            }
        }
        stage('Publish') {
            steps {
                echo("Publish stage!")
                script {
                    zip (
                        zipFile: "${ARTIFACT_NAME}",
                        archive: true,
                        dir: "pipeline/"
                        )
                    }
                rtUpload (
                    serverId: 'local-artifactory',
                    spec: ''' {
                        "files": [
                          {
                            "pattern": "${ARTIFACT_NAME}",
                            "target": "example-repo-local/"
                          }
                        ]
                    }'''
                    )
                }
            }
        }
        /*post {
            always {
                mail (
                    to: "jelenakretler@gmail.com",
                    subject: "always",
                    body: "mail that is always sent"
                )
            }
            success {
                mail (
                    to: "jelenakretler@gmail.com",
                    subject: "success",
                    body: "mail when job is success"
                )
            }
            failure {
                mail (
                    to: "jelenakretler@gmail.com",
                    subject: "failure",
                    body: "mail when failure occurred"
                )
            }
            unstable {
                mail (
                    to: "jelenakretler@gmail.com",
                    subject: "unstable",
                    body: "mail when job is unstable"
                )
            }
        }*/
    }
