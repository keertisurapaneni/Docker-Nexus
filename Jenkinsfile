node () {
  def container
  def NEXUS_URL = "192.168.99.1:8123"
    
    stage('Initialize'){
        def dockerHome = tool 'Docker'
        def mavenHome  = tool 'Maven 3.6.1'
        env.PATH = "${dockerHome}:${mavenHome}/bin:${env.PATH}"
    }

    stage('Clone repository') {
                checkout scm
    }
 

   stage('Build image') {
     //  container = docker.build('simplewebapp/test')
       sh 'docker build -t "simplewebapp/test1" .'
       }
  

    stage('Push image') {
      //docker.withRegistry('${NEXUS_URL}', '${NEXUS_CREDENTIAL_ID}') {
      //  container.push('latest')
      
       withCredentials([string(credentialsId: 'nexus-pwd', variable: 'nexusRepoPwd')]) {
         sh "docker login -u admin -p ${nexusRepoPwd} ${NEXUS_URL}"
     }
      sh 'docker tag simplewebapp/test1 192.168.99.1:8123/simplewebapp/test1'
    sh 'docker push 192.168.99.1:8123/simplewebapp/test1'
      echo "Success!"
 //     }
    }
}
