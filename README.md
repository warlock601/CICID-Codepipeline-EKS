# CICID-Codepipeline-EKS
To set up a CI process using AWS CodePipeline to build a Docker image from GitHub source code and then deploy it to Amazon EKS

Steps:
- Add the Dockerfile, Deployment.yml file, source code and everything else for this project in a Github repo.
- Step 2: Create ECR Repository
Open the Amazon ECR console.
 Create a new repository.
 Note down the repository URI.
- Step 3: Set Up IAM Roles and attach them the following policies:

        CodeBuild Role:
        
        
        AmazonEC2ContainerRegistryPowerUser <br>
        AmazonEKSClusterPolicy <br>
        AmazonEKSWorkerNodePolicy <br>
        AmazonEKSServicePolicy <br>
        
        
        EKS Role:
        
        AmazonEKSClusterPolicy <br>

- Step 4: Create CodeBuild Project
 Open the AWS CodeBuild console.
 Create a new project. And specify the buildspec file to be detected in the root directory. Name the file as "buildspec.yml" file and use the following pattern:

```bash
  version: 0.2

phases:
  pre_build:
    commands:
      - echo Logging in to Amazon ECR...
      - aws ecr get-login-password --region <your-region> | docker login --username AWS --password-stdin <your-ecr-repo-uri>
  build:
    commands:
      - echo Build started on `date`
      - echo Building the Docker image...
      - docker build -t <your-repo-name> .
      - docker tag <your-repo-name>:latest <your-ecr-repo-uri>:latest
  post_build:
    commands:
      - echo Pushing the Docker image...
      - docker push <your-ecr-repo-uri>:latest
artifacts:
  files:
    - k8s/deployment.yaml

```

Replace <your-region>, <your-ecr-repo-uri>, and <your-repo-name> with your actual values. <br>

- Step 5: Set Up EKS Cluster.
 Ensure your EKS cluster is set up and configured.
 Create a namespace if necessary.
- Step 6: Kubernetes Deployment Manifest.
 Update your deployment.yaml file with your ECR image URI and other configurations.
-Step-7: Create a Dewployment stage using CodeDeploy and use Lambda as compute (here I used Lmabda because of serverless capabilities).
-Step-8: Open the AWS CodePipeline console. <br>
Create a new pipeline.<br>
Add a source stage:
 Select GitHub and connect your repository.<br>
Add a build stage:
 Select AWS CodeBuild and choose the project created earlier.<br>
Add a deploy stage:
 Use AWS CodeDeploy


