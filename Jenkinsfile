pipeline{
    //Directives
    agent any
    tools {
        maven 'maven'
    }
    environment{
        ArtifactId = readMavenPom().getArtifactId()
        Version = readMavenPom().getVersion()
        Name = readMavenPom().getName()
        GroupId = readMavenPom().getGroupId()
    }

    stages {
        // Specify various stage with in stages

        // Stage 1: Build
        stage ('Build'){
            steps {
                sh 'mvn clean install package'
            }
        }

        // Stage 2: Testing
        stage ('Test'){
            steps {
                echo 'Testing...'

            }
        }

        // Stage 3: Publish the artefacts to Nexus
        stage ('Publish to Nexus'){
            steps {
                script {
                
                def NexusRepo = Version.endsWith("SNAPSHOT") ? "VinaysDevOpsLab-SNAPSHOT'" : "VinaysDevOpsLab-RELEASE"

                nexusArtifactUploader artifacts: 
                [[artifactId: "${ArtifactId}", 
                classifier: '', file: 
                "target/${ArtifactId}-${Version}.war", 
                type: 'war']], 
                credentialsId: '09e286d7-fda7-4ae3-b97f-d2e9831fa353', 
                groupId: "${GroupId}", 
                nexusUrl: '18.221.16.223:8081', 
                nexusVersion: 'nexus3', 
                protocol: 'http', 
                repository: "${NexusRepo}", 
                version: "${Version}"
             }
            }
        }

        // // Stage 4: Print some information
        stage ('Print envoronment variables'){
                    steps {
                        echo "Artifact ID is '${ArtifactId}'"
                        echo "Version is '${Version}'"
                        echo "Name is '${Name}'"
                        echo "Group ID is '${GroupId}'"
                    }
        }

        // // Stage 5: Deploying the build artifact to Apache Tomcat
        stage ('Deploy to Tomcat'){
            steps {
                echo 'Deploying...'
                sshPublisher(publishers: 
                [sshPublisherDesc(
                    configName: 'Ansible_Controller', 
                    transfers: [
                        sshTransfer(
                                cleanRemote: false,
                                execCommand: 'ansible-playbook /opt/playbooks/downloadanddeploy_as_root_user.yaml -i /opt/playbooks/hosts',
                            execTimeout: 120000
                        )
                    ], 
                    usePromotionTimestamp: false, 
                    useWorkspaceInPromotion: false, 
                    verbose: false)
                    ])
            }
        }
        
        // // Stage 6: Deploying the build artifact to Docker
        stage ('Deploy to Docker'){
            steps {
                echo 'Deploying...'
                sshPublisher(publishers: 
                [sshPublisherDesc(
                    configName: 'Ansible_Controller', 
                    transfers: [
                        sshTransfer(
                                cleanRemote: false,
                                execCommand: 'ansible-playbook /opt/playbooks/downloadanddeploy_docker.yaml -i /opt/playbooks/hosts',
                            execTimeout: 120000
                        )
                    ], 
                    usePromotionTimestamp: false, 
                    useWorkspaceInPromotion: false, 
                    verbose: false)
                    ])
            }
        }
        
        
    }

}