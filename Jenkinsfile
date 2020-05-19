node ('Ubuntu-appagent'){  
    def app
    stage('Cloning Git') {
        /* Let's make sure we have the repository cloned to our workspace */
       checkout scm
    }
    stage('SAST') {
        build 'Security-SAST-SNYK'
    }
       
    stage('Build-and-Tag') {
        //sh 'echo Build-and-Tag'
    /* This builds the actual image; synonymous to
         * docker build on the command line */
        app = docker.build("caiomarciox/snake")
    }
    stage('Post-to-dockerhub') {
        //sh 'echo Post-to-dockerhub'
        docker.withRegistry('https://registry.hub.docker.com', 'caiomarciox') {
            app.push("latest")
                 }
    stage('SECURITY-IMAGE-SCANNER') {
        build 'SECURITY-IMAGE-SCANNER-AQUAMICROSCAN'
    }
        
    stage('Pull-image-server') {
        //sh 'echo Pull-image-server'
         sh "docker-compose down"
         sh "docker-compose up -d"	
      }
    stage('DAST') {
        build 'SECURITY-IMAGE-SCANNER-AQUAMICROSCAN'
        }
}
}    
 

