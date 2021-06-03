/*
pipeline{
  agent any
  tools {
    terraform 'terraform'
  }



pipeline {
    agent {
        any {
            image 'hashicorp/terraform:latest'
            label 'LINUX-SLAVE'
            args  '--entrypoint="" -u root -v /opt/jenkins/.aws:/root/.aws'
        }
    }
  stages{
      stage('Terraform Init'){
        steps{
            sh label: '', script: 'terraform init'
        }
      }
      stage('Terraform Plan'){
        steps{
          sh label: '', script: 'terraform plan'
        }
      }
  }
}

/*
pipeline{
  agent {
        docker {image 'hashicorp/terraform:light'}
    }
  stages{
    stage('terraform init'){
      steps{
        sh "terraform init"
      }
    }
  }
}*/
// Jenkinsfile
/*
pipeline{
  agent any
  tools {
    terraform 'terraform'
  }*/



try {
  stage('checkout') {
    node {
      cleanWs()
      checkout scm
    }
  }

  // Run terraform init
  stage('init') {
    node {
      {
        ansiColor('xterm') {
          sh 'terraform init'
        }
      }
    }
  }

  // Run terraform plan
  stage('plan') {
    node {
      {
        ansiColor('xterm') {
          sh 'terraform plan'
        }
      }
    }
  }

  if (env.BRANCH_NAME == 'main') {

    // Run terraform apply
    stage('apply') {
      node {
        {
          ansiColor('xterm') {
            sh 'terraform apply -auto-approve'
          }
        }
      }
    }

    // Run terraform show
    stage('show') {
      node {
      
       {
          ansiColor('xterm') {
            sh 'terraform show'
          }
        }
      }
    }
  }
  currentBuild.result = 'SUCCESS'
}
catch (org.jenkinsci.plugins.workflow.steps.FlowInterruptedException flowError) {
  currentBuild.result = 'ABORTED'
}
catch (err) {
  currentBuild.result = 'FAILURE'
  throw err
}
finally {
  if (currentBuild.result == 'SUCCESS') {
    currentBuild.result = 'SUCCESS'
  }
}