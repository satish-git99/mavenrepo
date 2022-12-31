pipeline{

    agent any

    triggers {
            pollSCM '* * * * *'
    }
    stages{

        stage('GitCheckout'){

            steps{

                checkout scmGit(branches: [[name: '*/feat01']], extensions: [], userRemoteConfigs: [[credentialsId: '4d4e2498-2476-47db-8f1b-11d9a45905af', url: 'https://github.com/satish-git99/mavenrepo.git']])
            }

        }
        stage('MavenBuild'){

            steps{

                sh 'mvn package'
            }

        }
        stage('SonarQubeTesting'){

            steps{

                withSonarQubeEnv('sonar'){
                sh 'mvn sonar:sonar'
                }
            }

        }
        stage('NexusArtifactUploader'){
            steps{

                nexusArtifactUploader artifacts: [[artifactId: 'studentapp', classifier: '', file: 'pom.xml', type: 'pom']], credentialsId: '7ba33b30-49d7-464c-a454-1bc78a0481d3', groupId: 'com.jdevs', nexusUrl: '43.204.228.118:8081/', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-snapshots', version: '2.5-SNAPSHOT'
            }
        }
        stage('DeployToTomcatServer'){
            steps{

                deploy adapters: [tomcat8(credentialsId: 'ec84313c-aba8-4aca-b04f-fbcc3eef1e91', path: '', url: 'http://52.66.198.235:8080/')], contextPath: null, war: 'target/*war'
            }
        }
        stage('EmailNotification'){
            steps{
        
                emailext body: 'status', subject: 'pipeline job', to: 'vempalasatishkumar9@gmail.com'
            }
        }
        stage('SlackNotification'){
            steps{
                slackSend channel: 'jenkins'
            }
        }
    }

}
