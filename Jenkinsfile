pipeline {
    agent { label 'JDK8_17_Maven' }
    stages {
        stage ('Clone') {
            steps {
                git url: 'https://github.com/dumyrepositories/spring-petclinic1.git',
                    branch: 'main'
            }
        }

        stage ('Artifactory configuration') {
            steps {

                rtMavenDeployer (
                    id: "MAVEN_DEPLOYER",
                    serverId: "Jfrog_Cloud",
                    releaseRepo: 'test5-libs-release',
                    snapshotRepo: 'test5-libs-snapshot'
                )

                rtMavenResolver (
                    id: "MAVEN_RESOLVER",
                    serverId: "Jfrog_Cloud",
                    releaseRepo: 'test5-libs-release-local',
                    snapshotRepo: 'test5-libs-snapshot-local'
                )
            }
        }

        stage ('Exec Maven') {
            tools {
                jdk 'JDK17'
            }
            steps {
                rtMavenRun (
                    tool: 'MAVEN_DEFAULT',
                    pom: 'pom.xml',
                    goals: 'clean install',
                    deployerId: "MAVEN_DEPLOYER",
                    resolverId: "MAVEN_RESOLVER"
                )
            }
        }

        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "Jfrog_Cloud"
                )
            }
        }
    }
}