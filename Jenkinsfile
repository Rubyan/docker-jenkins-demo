node {
   def commit_id
   
   stage('preparation') {
     checkout scm
     sh "git rev-parse --short HEAD > .git/commit-id"
     commit_id = readFile('.git/commit-id').trim()
   }

   stage('test') {
     def myTestContainer = docker.image('python:2.7.14')
     myTestContainer.pull()
     myTestContainer.inside {
       sh 'pip install -U pytest'
       sh 'pytest'
     }
   }
                                    
   stage('docker build/push') {            
     docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
       def app = docker.build("rubyan/docker-jenkins-demo:${commit_id}", '.').push()
     }                                     
   }                                       
}     