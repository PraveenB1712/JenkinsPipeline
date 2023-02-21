pipeline{
 // first stage   
    tools{
        jdk 'myjava'
        maven 'mymaven'
    }
 // Agent section is mandatory   
    agent any
    
    stages{
        
        stage('Clone the repo')
        {
            steps{
                git 'https://github.com/devops-edu23/samplejavaapp.git'
            }
        }
       // compile stage
        stage('Compile the code')
        {
            steps{
                sh 'mvn compile'
            }
        }
        stage('Code Review')
        {
            steps{
                sh 'mvn pmd:pmd'
            }
        }
        
        stage('Unit Testing')
        {
            steps{
                sh 'mvn test'
            }
            post{
                success{
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        
        stage('Package')
        {
            steps{
            sh 'mvn package'
            }
        }
       
         stage('build & push docker image') {
	         steps {
              withDockerRegistry(credentialsId: 'DOCKER_HUB_LOGIN', url: 'https://index.docker.io/v1/') {
                    sh script: 'cd  $WORKSPACE'
                    sh script: 'docker build --file Dockerfile --tag docker.io/praveenbiradar1/jenkinsjavaapp:$BUILD_NUMBER .'
                    sh script: 'docker push docker.io/praveenbiradar1/jenkinsjavaapp:$BUILD_NUMBER'
              }	
           }		
        }
        stage('deploy-QA') {
	         steps {
                    sh script: 'sudo ansible-playbook --inventory /tmp/myinv $WORKSPACE/deploy/deploy-kube.yml --extra-vars "env=qa build=$BUILD_NUMBER"'
           }		
        }
    }    
   
}
