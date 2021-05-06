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
                script {
                
                def NexusRepo = Version.endsWith("SNAPSHOT") ? "VinaysDevOpsLab-SNAPSHOT'" : "VinaysDevOpsLab-RELEASE"

                nexusArtifactUploader artifacts: 
                [[artifactId: "${ArtifactId}", 
                classifier: '', 
                file: "target/${ArtifactId}-${Version}.war", 
                type: 'war']], 
                credentialsId: 'b7db6489-daa3-4be1-98d2-fbd37cec165e', 
                groupId: "${GroupId}", 
                nexusUrl: '3.15.161.249:8081', 
                nexusVersion: 'nexus3', 
                protocol: 'http', 
                repository: "${NexusRepo}",
                version: "${Version}"
             }
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