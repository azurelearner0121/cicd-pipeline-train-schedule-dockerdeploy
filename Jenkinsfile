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
      archiveArtifacts artifacts: 'dist/trainSchedule.zip   
      }
    }
    stage('BuildDocker'){
      steps{
    echo 'BuildingDockerImage'
      }
    
    } 
  
  }
}
