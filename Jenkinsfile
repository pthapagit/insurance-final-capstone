node {
    
    stage('Git Checkout') {
        git branch: 'main', url: 'https://github.com/pthapagit/insurance-final-capstone.git'
    }
    
    stage('maven clean build and package')
        sh 'mvn clean package'
    
    stage('containerize application'){
        sh 'docker build -t pthapa97/insure-me:1.0 .'
    }
    
    stage('release image to docker hub'){
        withCredentials([string(credentialsId: 'DockerHubPwd', variable: 'DockerHubPwd')]) {
    // some block
        sh 'docker login -u pthapa97 -p ${DockerHubPwd}'
        sh 'docker push pthapa97/insure-me:1.0'
        }
    }
    
    stage('deploy to test server with ansible'){
        ansiblePlaybook become: true, credentialsId: 'ansible-key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'configure-test-server.yml', vaultTmpPath: ''
    }
    
    stage('Start test-app build'){
        build 'capstone-test-app'
    }
    
    stage('start prod build'){
        build 'Final-Prod-Server'
    }
    
}
