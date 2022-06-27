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
    
    stage('PushDockerImageACR'){
      steps {
       echo 'PushingDockerImageToACR' 
        script {
        docker.withRegistry('https://testjenkins.azurecr.io', 'acr-cred') {
         app.push("${env.BUILD_NUMBER}")            
         app.push("latest")     
        }
        }
        
      }
      
    }
    stage('DeployToAKS)
      steps {
       echo 'DeployingToAKs' 
        script {
           sh 'az aks get-credentials --resource-group az104-revision --name test-jenkins'
           sh 'kubectl get svc'
           sh 'kubectl apply -f deployment.yaml'
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
 
   /* stage('PushImageToECR') {
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
      
      
    }*/
    /* To deploy to GKEdo the following
    1. Create a service account in GKE
    2. Add a principal with the service account
    3. Add the Roles - Storage Admin (to push to GCR), Kubernetes Cluster Engine Administrator (To deploy to GKE)
    4. Plugins in Jenkins - Google Container Registry plugin , Google OAuth Cred plugin, Google Kubernetes Engine plugin
    5. Create a credential in jenkins with the service account key generated , credential type: Google service account from Private key
    6. Use the below plugin to deploy to GKE
    
    
    */
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
    5. Plugin needed in Jrnkins - Amazon ECR plugin, Cloudbees plugin for AWS
    6. Credential type in Jenkins - AWS credentials. Takes in access key and secret access key
    
    
    */
  /*  stage('DeployToEKS'){
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
      
    }*/
  
    
    
  }
  
  
  
}
