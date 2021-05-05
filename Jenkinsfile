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

        // stage 1. Build
        stage ('Build'){
            steps {
                sh 'mvn clean install package'
            }
        }

        // Stage2 : Testing
        stage ('Test'){
            steps {
                echo ' testing......'

            }
        }

        // Stage3 : Publish the artefacts to Nexus
        stage ('Publish to Nexus'){
            steps {
                nexusArtifactUploader artifacts: 
                [[artifactId: '${ArtifactId}', 
                classifier: '', 
                file: 'target/VinayDevOpsLab-0.0.4-SNAPSHOT.war', 
                type: 'war']], 
                credentialsId: 'fafa363d-a6a6-4840-a019-113d0a2307b0', 
                groupId: '${GroupId}', 
                nexusUrl: '3.128.170.90:8081/', 
                nexusVersion: 'nexus3', 
                protocol: 'http', 
                repository: 'VinaysDevOpsLab-SNAPSHOT', 
                version: '${Version}'
            }
        }

        // // Stage4 : Print some information
        stage ('Print envoronment variables'){
                    steps {
                        echo "Artifact ID is '${ArtifactId}'"
                        echo "Version is '${Version}'"
                        echo "Name is '${Name}'"
                        echo "Group ID is '${GroupId}'"
                    }
        }

        // // Stage5 : Deploying
        stage ('Deploy'){
            steps {
                echo 'deploying.....'
            }
        }

        
        
    }

}