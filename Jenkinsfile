pipeline {

  environment {
    app = "webapp"
    environment = "dev"
    namespace = "dev"
  }
  agent any

    stages {

      stage ('Checkout SCM'){
        steps {
          checkout([$class: 'GitSCM', branches: [[name: '*/dev']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'git', url: 'https://dptrealtime@bitbucket.org/dptrealtime/loginapp-cicd-deployment.git']]])
        }
      }
	  
	  stage ('Deploy Helm Charts')  {
	      steps {
         
           dir("charts"){
             withCredentials([usernamePassword(credentialsId: 'jfrog', usernameVariable: 'username', passwordVariable: 'password')]) {
                    sh 'sudo /usr/local/bin/helm repo add dft01-helm-local  https://dpt4hub.jfrog.io/artifactory/dft01-helm-local --username $username --password $password'
                    sh "sudo /usr/local/bin/helm repo update"
                    sh "sudo /usr/local/bin/helm upgrade ${app}-${environment} --install --namespace ${namespace} --force -f values.yaml ."
                    sh "sudo /usr/local/bin/helm list -a --namespace ${namespace}"
                }
           }
        }
         
      }
         
   } 
}