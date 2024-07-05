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
 Create a new project with the following buildspec file:


