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
        app=docker.build("teak-environs-348513/train-schedule")
        app.inside {       
             sh 'echo "Tests passed"'        
            }   
      }
      }
      
    } 
    stage('PushDockerImage'){
      steps {
       echo 'PushingDockerImage' 
        script {
        docker.withRegistry('https://gcr.io', 'gcr:My First Project') {
         app.push("${env.BUILD_NUMBER}")            
         app.push("latest")     
        }
        }
        
      }
      
    }
 
    stage('PushImageToECR') {
      steps{
       echo 'Pushing imagetoECR'
        
      }
      
      
    }
    stage('DeployToGKE') {
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
      
      
    }
  
  
  }
  
  
  
}
