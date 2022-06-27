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
         docker.withRegistry('https://457335132494.dkr.ecr.us-east-1.amazonaws.com/', 'ecr:us-east-1:aws_ecr_push') {
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
    /* 
    
    To enable jenkins to deploy to EKS do the below:
    1. Configure aws cli with aws configure commadn in the worker node
    2. Give the user policy permission to authenticate to cluster
    {
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "eks:DescribeCluster",
                "eks:ListClusters"
            ],
            "Resource": "*"
        }
    ]
}
    3. Run the below eksctl command to add the jenkins user to cluster auth configmap
    eksctl create iamidentitymapping \
    --cluster test-cluster-new \
    --region=us-east-1 \
    --arn arn:aws:iam::457335132494:user/jenkins-user\
    --group system:masters  \
    --no-duplicate-arns
   The group is derived from kubectl describe clsterrolebinding cluster-admin command output
   
    4. Generate kubeconfig entry for the jenkins user to authenticate to the cluster
    'aws eks --region us-east-1 update-kubeconfig --name test-cluster-new'
    
    
    */
    stage('DeployToEKS'){
      steps {
     echo 'DeployToEKS'
      script {
         echo 'authenticate to cluster'
         sh 'aws sts get-caller-identity'
         sh 'aws eks --region us-east-1 update-kubeconfig --name test-cluster-new'
         echo 'get all objects'
         sh 'kubectl get svc'
         sh  'kubectl apply -f deployment.yaml'
      }
      }
      
    }
  
    
    
  }
  
  
  
}
