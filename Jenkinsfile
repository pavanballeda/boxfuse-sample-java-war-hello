pipeline {
    agent any
    stages {
        stage('build_number') {
        steps {
            buildName '#pipeline-${BULID_NUMBER}'
            script{
                currentBuild.displayName ="#pipeline-${BUILD_NUMBER}"
            }
        }
    } 
        stage('clone') {
            steps {
               git credentialsId: '7fb1a87f-97b0-40cb-a4c8-6584c4c04407', url: 'https://github.com/pavanballeda/boxfuse-sample-java-war-hello.git' 
            }
        }
        stage('build-maven') {
            steps {
                bat 'mvn clean install'
            }
        }
        
        stage('deploy-tomcatserver') {
            steps {
                deploy adapters: [tomcat8(credentialsId: 'e3b16a95-c8db-4769-b263-ee6185bd4110', path: '', url: 'http://localhost:8090')], contextPath: 'pipeline_job', war: '**/*.war'
            }
        }
        stage('store-nexus') {
            steps {
             nexusPublisher nexusInstanceId: 'nexus', nexusRepositoryId: 'Maven-new', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: 'target/hello-1.0.war']], mavenCoordinate: [artifactId: 'pipeline', groupId: 'com.new-project', packaging: 'war', version: '1.1.1']]]   
            }
        }
    }
}
