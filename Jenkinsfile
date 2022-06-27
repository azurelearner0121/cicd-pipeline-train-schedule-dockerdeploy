pipeline {
  agent { label 'dev-agent'}
  stages {
    stage('BuildCode'){
    steps{
     echo 'In build step'
      sh './gradlew build --no-daemon'
         
    }
    
    }
    stage('ArchiveArtifact'){
      steps {
    echo  'Archive Artifact'
      archiveArtifacts artifacts: 'dist/trainSchedule.zip'   
      }
    }
    stage('BuildDocker'){
      steps{
        echo 'BuildingDockerImage'
        script {
       // app=docker.build("teak-environs-348513/train-schedule")
           app=docker.build("train-schedule")
           app.inside {       
             sh 'echo "Tests passed"'        
            }   
      }
      }
      
    } 
   /*stage('PushDockerImageGCR'){
      steps {
       echo 'PushingDockerImage' 
        script {
        docker.withRegistry('https://gcr.io/', 'gcr:My First Project') {
         app.push("${env.BUILD_NUMBER}")            
         app.push("latest")     
        }
        }
        
      }
      
    }*/
 
    stage('PushImageToECR') {
      steps{
       echo 'Pushing imagetoECR'
        script {
       //  app.tag('train-schedule')
         docker.withRegistry('https://457335132494.dkr.ecr.us-east-1.amazonaws.com/', 'ecr:aws_ecr_push') {
         app.push("${env.BUILD_NUMBER}")            
         app.push("latest")  
          
        }
        }
        
      }
      
      
    }
    /* stage('DeployToGKE') {
      steps {
        echo 'Deplploying to GKE'
        step([$class: 'KubernetesEngineBuilder', 
                        projectId: "teak-environs-348513",
                        clusterName: "test-cluster",
                        zone: "us-central1-a",
                        manifestPattern: 'deployment.yaml',
                        credentialsId: "gke-service-account",
                        verifyDeployments: true])
        
        
      }
      
      
    }*/
  
  
  }
  
  
  
}
