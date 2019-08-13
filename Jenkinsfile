node () {
  def container
 // def  dockerTool = tool name: 'Docker', type: 'org.jenkinsci.plugins.docker.commons.tools.DockerTool'
    environment {
        // This can be nexus3 or nexus2
        NEXUS_VERSION = "nexus3"
        // This can be http or https
        NEXUS_PROTOCOL = "http"
        // Where your Nexus is running
        NEXUS_URL = "http://192.168.99.1:8123"
        // Repository where we will upload the artifact
        NEXUS_REPOSITORY = "test"
        // Jenkins credential id to authenticate to Nexus OSS
        NEXUS_CREDENTIAL_ID = credentials('NexusID')
    }
    
    stage('Initialize'){
        def dockerHome = tool 'Docker'
        def mavenHome  = tool 'Maven 3.6.1'
        env.PATH = "${dockerHome}:${mavenHome}/bin:${env.PATH}"
    }

    stage('Clone repository') {
                checkout scm
    }
 

   stage('Build image') {
       // container = docker.build('simplewebapp/test')
       sh 'docker build -t "simplewebapp/test1" .'
       }
  

    stage('Push image') {
      //docker.withRegistry('${NEXUS_URL}', '${NEXUS_CREDENTIAL_ID}') {
      //  container.push('latest')
       withCredentials([string(credentialsId: 'nexus-pwd', variable: 'nexusRepoPwd')]) {
        sh "docker login -u admin -p ${nexusRepoPwd} 192.168.99.1:8123"
     }
      sh 'docker tag simplewebapp/test1 192.168.99.1:8123/simplewebapp/test1'
    sh 'docker push 192.168.99.1:8123/simplewebapp/test1'
      echo "Success!"
 //     }
    }
}
