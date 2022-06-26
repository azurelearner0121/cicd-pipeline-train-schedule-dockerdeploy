pipeline {
  agent { label 'dev-agent'}
  stages {
    stage('BuildCode'){
    steps{
     echo 'In build step'
      sh ''./gradlew build --no-daemon'
         
    }
    
    }
    stage('ArchiveArtifact'){
    echo  'Archive Artifact'
      archiveArtifacts artifacts: 'dist/trainSchedule.zip   
    
    }
    stage('BuildDocker'){
    echo 'BuildingDockerImage'
    
    
    } 
  
  }
}


