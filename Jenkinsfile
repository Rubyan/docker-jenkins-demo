node {
   def commit_id

   stage('preparation') {
     checkout scm
     sh "git rev-parse --short HEAD > .git/commit-id"
     commit_id = readFile('.git/commit-id').trim()
   }

   stage('test') {
     def myTestContainer = docker.image('rubyan/hello:latest')
     myTestContainer.pull()
     myTestContainer.inside {
       sh 'pytest'
     }
   }
                                    
   stage('docker build/push') {            
     docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
       def app = docker.build("rubyan/docker-jenkins-demo:${commit_id}", '--build-args http_proxy=$http_proxy .').push()
     }                                     
   }                                       
}     
