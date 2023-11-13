node{
        stage('git checkout'){
            echo "checking out the code from github"
            git 'https://github.com/skdeepthi/insureme-capstone.git'
        }
        
        stage('maven build'){
            sh 'mvn clean package'
        }
        
        stage('build docker image'){
            sh 'docker build -t deepthi/insure-me:1.0 .'
        }
        
        stage('push docker image to docker hub registry')
        {
            echo 'pushing images to registry'
            
             withCredentials([string(credentialsId: 'dockerhub-password', variable: 'dockerHubpassword')]) {
    // Docker Credentials block
     sh "docker login -u deepthisk -p ${dockerHubpassword}"
     sh 'docker push deepthisk/insure-me:1.0'
            }
        }
        
        stage('configure test-server and deploy insure-me'){
            echo "configuring test-server"
          //  sh 'ansible-playbook configure-test-server.yml'
            ansiblePlaybook credentialsId: 'ssh-connection-ansible', disableHostKeyChecking: true, installation: 'Ansible', inventory: '/etc/ansible/hosts', playbook: 'configure-test-server.yml', vaultTmpPath: ''
    
              }
        
}
