## Workshop Objective
In this workshop, you will learn how you can:
1. Deploy a Generative AI chatbot application on Kubernetes by deploying a vLLM and a WebUI Pod on an Amazon EKS cluster, store and access the Mistral-7B model using Amazon FSx for Lustre and  Amazon S3, and leverage Accelerate Compute for your Generative AI workload using AWS Inferentia Accelerator.
2. Use Karpenter to scale the number of EKS nodes, when there are additional Pod requests that require additional nodes, to enable scale and operational efficiency.
3. Use AWS Inferentia Accelerated Compute in your Amazon EKS clusters, as a new nodepool to power your Generative AI applications.
4. Configure Amazon FSx for Lustre and Amazon S3, as your performant and scalable data layer, which will host your model and data
5. Achieve operational efficiency at the data layer: accessing the same model data across container Pods without storing multiple copies, and seamlessly sharing your data across regions, for scenario's such as distributed access and sharing, to DR.


****Target Audience****: DevOps engineers, Machine Learning Scientists/Engineers, Container & Storage engineers, Cloud Architects

****Prerequisites****: Recommended to have an fundamental understanding of AWS containers, and AWS Cloud

****Duration****: Approximately take 2 hours.

![lab-image](/static/images/lab-image.png)

-----

## Generative AI and Machine Learning
Generative AI and Machine Learning (ML) is helping businesses transform the way they operate and innovate. Generative AI refers to a class of Artificial Intelligence that leverages Large Language Models (LLM) in order to generate new content from a prompt, content such as text, images, audio, and software code.

## What is a Large Language Model (LLM)
Large Language Models (LLMs) are a type of machine learning model that is trained on vast amounts of text data to learn the patterns and structure of natural language. These models can then be used for a wide range of natural language processing tasks, such as text generation, question answering, and language translation. In this lab we are going to use the open-source Mistral-7B-Instruct model, which is a specific LLM model with 7 billion parameters. The "Instruct" in the name refers to the fact that this model has been trained to follow instructions and perform a wide variety of tasks, beyond just generating text, i.e. it is suitable for chat applications. You will be using this open source LLM model in this workshop.


## What is vLLM
[**vLLM (Virtual Large Language Model)**](https://github.com/vllm-project/vllm) is an open-source, easy-to-use, library for LLM inference and serving. It provides a framework that allows LLM models such as Mistral-7B-Instruct, to be deployed to provide text generation inference. vLLM provides an API that is compatible with OpenAI API, making it easy to integrate LLM applications.

**vLLM is fast with:**
- State-of-the-art serving throughput
- Efficient management of attention key and value memory with PagedAttention
- Continuous batching of incoming request
- Fast model execution with CUDA/HIP graph

**vLLM is flexible and easy to use with:**
- Seamless integration with popular HuggingFace models
- OpenAI-compatible API server
- Prefix caching support
- Supports chipsets such as: AWS Neuron, NVIDIA GPUs and others,

## Deploying Mistral-7B-Instruct using a vLLM on Amazon EKS
To provide text generation inference capability with an OpenAI-compatible endpoint, we will deploy the Mistral-7B-Instruct model using the vLLM framework on Amazon Elastic Kubernetes Service (EKS). We will use Karpenter to spin up the AWS inferentia2 EC2 node (Accelerated Compute designed for Generative AI), where it will launch a vLLM Pod from an container image.

## What is Amazon EKS (Elastic Kubernetes Service)
[**Amazon EKS**](https://aws.amazon.com/eks/), is a managed service that makes it easy for you to deploy, run, manage and scale container based apps using Kubernetes on AWS, without installing and operating your own Kubernetes control plane or worker nodes. Amazon EKS clusters can scale to support thousands of containers, which makes it ideal for Generative AI and ML workloads, where you can tune and deploy LLMs on Amazon EKS. Amazon EKS serves as an effective orchestrator to help achieve rapid scale out and scale in that is required for Generative AI and ML workloads, optimal cost efficiency.

## How to consume the Inference Service
You can connect to the Inference Service using the **"Open WebUI"** application, which is designed to consume the OpenAI-compatible endpoint provided by the vLLM-hosted Mistral-7B-Instruct model that you will deploy in the workshop. The Open WebUI application allows users to interact with the LLM model through a chat-based interface. To use the Open WebUI application, simply deploy the application container, and connect to the WebUI URL that is provided and start chatting with the LLM model. The WebUI application will handle the communication with the VLLM-hosted Mistral-7B-Instruct model, providing a seamless user experience


## Storing and accessing your model and training data
In this workshop the **Mistral-7B-Instruct** model is stored in an Amazon S3 bucket [**Amazon S3**](https://aws.amazon.com/s3/), which is linked to an  [**Amazon FSx for Lustre File system S3**](https://aws.amazon.com/fsx/lustre/). The vLLM container will consume the Mistral model data via the mounted Amazon FSx for Lustre instance for the Generative AI Chat application. Amazon FSx for Lustre is a fully managed service that provides a high-performance scalable file system, for workloads where speed matters, providing sub-millisecond latency, and scaling to TB/s of throughput and millions of IOPS. Amazon FSx also integrates with Amazon S3 (highly durable, available and scalable object store), making it easy for you to store, access and process vast amounts of cloud data with the Lustre high-performance file system.

## Accelerating your Compute
 [**AWS Inferentia accelerators**](https://aws.amazon.com/machine-learning/inferentia/) are designed by AWS to deliver high performance at the lowest cost in Amazon EC2 for your deep learning (DL) and generative AI inference applications, where Inferentia2-based Amazon EC2 Inf2 instances are optimized to deploy increasingly complex models, such as large language models (LLM). [**AWS Neuron SDK**](https://aws.amazon.com/machine-learning/neuron/) is an SDK with a compiler, runtime, and profiling tools that unlocks high-performance and cost-effective deep learning (DL) acceleration. AWS Neuron SDK helps developers deploy models on the AWS Inferentia accelerators, where it integrates natively with popular frameworks, such as PyTorch and TensorFlow, so that you can continue to use your existing code and workflows and run on Inferentia accelerators.


## Repo structure

```bash
.
├── README.md
├── assets
│   └── README.md
├── content
│   ├── 010_introduction
│   │   └── index.en.md
│   ├── 020_setup
│   │   ├── 021_on_demand
│   │   │   └── index.en.md
│   │   ├── 022_aws_event
│   │   │   └── index.en.md
│   │   └── index.en.md
│   ├── 030_module_explore_karpenter
│   │   └── index.en.md
│   ├── 100_module1_eks_fsxl
│   │   ├── 110_DeployAmazonFSxLustreCSIDriverToEKS.md
│   │   ├── 120_StaticProvisioning.md
│   │   ├── 123_ViewFSxConsole.md
│   │   └── index.en.md
│   ├── 200_module2_genai
│   │   ├── 210_Deploy.md
│   │   ├── 220_webui.md
│   │   └── index.en.md
│   ├── 300_module3_replication
│   │   ├── 320_CreateCrossRegionReplicationForS3Buckets.md
│   │   ├── 330_RegionalFailoverAndSwitch.md
│   │   └── index.en.md
│   ├── 400_module1_fsx_perf
│   │   ├── 422_DynamicProvisioning.md
│   │   ├── 430_Fio_performanceTesting.md
│   │   └── index.en.md
│   └── index.en.md
└── static
    ├── GenAIFSXWorkshopOnEKS.yaml
    ├── download
    │   ├── check.yaml
    │   ├── download-upload.yaml
    │   ├── s3-upload.json
    │   ├── sysprep-new.yaml
    │   ├── sysprep-nodepool.yaml
    │   └── sysprep.yaml
    ├── eks
    │   ├── FSxL
    │   │   ├── fsxL-claim.yaml
    │   │   ├── fsxL-dynamic-claim.yaml
    │   │   ├── fsxL-persistent-volume.yaml
    │   │   ├── fsxL-storage-class.yaml
    │   │   ├── pod.yaml
    │   │   └── pod_performance.yaml
    │   └── genai
    │       ├── inferentia_nodepool.yaml
    │       ├── mistral-fsxl.yaml
    │       └── open-webui.yaml
    ├── images
    │   ├── [ workshop images .. ]
    │   
    │   └── vllm_pod_1.png
    ├── scripts
    │   ├── cleanup.sh
    │   ├── install.sh
    │   └── sysprep.sh
    └── terraform
        ├── helm-values
        │   ├── kube-prometheus.yaml
        │   └── nvidia-values.yaml
        └── main.tf
```


### Part 1 : Prerequisite of setting up On-demand Workshop
Here you will do pre-setup before launching cloud formation stack which will provision your workshop. 

### NOTE:
Here some of step you may feel as duplication of data, however its to align it with sponsored workshop setup and code managability. 


1. You will need ec2 jump box where you can run these commands with needful permissions in your account. We are not provide detil steps to provision EC2 because each account may be differently managed. 

Please create EC2 instance where you should have awscli, docker and git commands available, if not then you can install them using instuctions here on Amazon Linux 2, if you have differnt OS then please find instuctions to install these commands.

- awscli : https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```
- Docker : 

```bash
sudo yum update -y
sudo yum install -y docker
sudo service docker start
sudo usermod -a -G docker participant
docker ps
```

- Git : 
```bash
sudo yum update -y
sudo yum install git -y
git — version
git config — global user.name “Your Name”
git config — global user.email “your_email@example.com”
```

2. Git Clone : 

```bash
git clone https://github.com/git4example/genai-fsx-workshop-on-eks.git
```

3. Create s3 bucket for temporary hosting workshop asseets. These asset bucket should be in the same region as of your CFN stack. Note that some of the automation in CFN stack and terraform executing as part of setup will copy over these data to vscode instance and new s3 bucket required to upload/host the workshop data and GenAI model. 

```bash
export REGION=< your-region >
ASSET_BUCKET=< new-bucket-name >
aws s3api create-bucket --bucket $ASSET_BUCKET --region $REGION
```

4. Move needful code to your asset bucket which we will be using for the provisioning resources using CloudFormation in next step. 

```bash
aws s3 sync ./genai-fsx-workshop-on-eks ${ASSET_BUCKET}/genai-fsx-workshop-on-eks
```


5. : Download model 
```bash
docker run -v ./work-dir/:/work-dir/ --entrypoint huggingface-cli public.ecr.aws/parikshit/huggingface-cli:slim download "enghwa/neuron-mistral7bv0.2" --local-dir /work-dir/Mistral-7B-Instruct-v0.2
```

6. Upload model to asset bucket. In following command replace credentials to allow access to assets bucket.

```bash
export $(printf "AWS_ACCESS_KEY_ID=%s exp=%s AWS_SESSION_TOKEN=%s" $(aws sts assume-role --role-arn <role-arn> --role-session-name <session-name> --query "Credentials.[AccessKeyId,SecretAccessKey,SessionToken]" --output text))
```

```bash
docker run -e AWS_DEFAULT_REGION=$REGION \
  -e AWS_ACCESS_KEY_ID=$AWS_ACCESS_KEY_ID \
  -e AWS_SECRET_ACCESS_KEY="<access-key>" \
  -e AWS_SESSION_TOKEN=$AWS_SESSION_TOKEN \
  -v ./work-dir/:/work-dir/  public.ecr.aws/parikshit/s5cmd cp /work-dir/Mistral-7B-Instruct-v0.2/ s3://<your-bucket>/Mistral-7B-Instruct-v0.2/
```


### Part 2 : Provision workshop resources

This will take upto 40 - 60 mins. 

Any stack creation/deletion failures can be investigated by looking at Cloudformation stack along with `/aws/lambda/GenAIFSXWorkshopOnEKS-XXX` and `/aws/ssm/GenAIFSXWorkshopOnEKS-XXX` log groups in AWS Cloudwatch Logs


Validate Template : 
```bash
aws cloudformation validate-template --template-url https://${ASSET_BUCKET}.s3.amazonaws.com/genai-fsx-workshop-on-eks/static/GenAIFSXWorkshopOnEKS.yaml
```

Set parameter values : 
```bash
export REGION=${REGION}
STACK_NAME=GenAIFSXWorkshopOnEKS
VSINSTANCE_NAME=VSCodeServerForEKS
ASSET_BUCKET_ZIPPATH=""
ASSET_BUCKET=${ASSET_BUCKET}
ASSET_BUCKET_PATH=genai-fsx-workshop-on-eks
```

Create stack : 
```bash
aws cloudformation create-stack \
  --stack-name ${STACK_NAME} \
  --template-url https://${ASSET_BUCKET}.s3.amazonaws.com/GenAIFSXWorkshopOnEKS.yaml \
  --region $REGION \
  --parameters \
  ParameterKey=VSCodeUser,ParameterValue=participant \
  ParameterKey=InstanceName,ParameterValue=${VSINSTANCE_NAME} \
  ParameterKey=InstanceVolumeSize,ParameterValue=100 \
  ParameterKey=InstanceType,ParameterValue=t4g.medium \
  ParameterKey=InstanceOperatingSystem,ParameterValue=AmazonLinux-2023 \
  ParameterKey=HomeFolder,ParameterValue=environment \
  ParameterKey=DevServerPort,ParameterValue=8081 \
  ParameterKey=AssetZipS3Path,ParameterValue=${ASSET_BUCKET_ZIPPATH} \
  ParameterKey=BranchZipS3Path,ParameterValue="" \
  ParameterKey=FolderZipS3Path,ParameterValue="" \
  ParameterKey=C9KubectlVersion,ParameterValue=1.30.2 \
  ParameterKey=C9NodeViewerVersion,ParameterValue=latest \
  ParameterKey=EKSClusterName,ParameterValue=eksworkshop \
  ParameterKey=EKSClusterVersion,ParameterValue=1.30 \
  ParameterKey=ParticipantAssumedRoleArn,ParameterValue=NONE \
  ParameterKey=ParticipantRoleArn,ParameterValue=NONE \
  ParameterKey=ParticipantRoleArn,ParameterValue=NONE \
  ParameterKey=Assets,ParameterValue=s3://${ASSET_BUCKET}/${ASSET_BUCKET_PATH}/assets/ \
  --disable-rollback \
  --capabilities CAPABILITY_NAMED_IAM
```

### Part 3 : Access your workshop

**Connect to your AWS lab environment via Open source VSCode IDE**

Ref : [code-server](https://github.com/coder/code-server) 

You will be using the Open source VSCode IDE terminal to copy and paste commands that are provided in this workshop. 

::alert[Note: Please use google chrome browser for best user experience. Firefox may experience some issues while copy-paste commands.]{header="Important" type="warning"}

1. Go to Cloud formation console [link](https://console.aws.amazon.com/cloudformation) and select `genaifsxworkshoponeks` stack 
2. Go to Stack **Outputs**
3. Copy Password and click URL
4. Enter copied password in the new tab opened for the URL


![CFN-Output](/static/images/cfn-output.png)

5. Select your VSCode UI theam 

![Select Theme](/static/images/select-theme.png)

6. You can maximize terminal window.

![maximize](/static/images/maximize.png)

### Validate the IAM role {#validate_iam}

- Use the [GetCallerIdentity](https://docs.aws.amazon.com/cli/latest/reference/sts/get-caller-identity.html) CLI command to validate that the VSCode IDE is using the correct IAM role.

```bash
aws sts get-caller-identity
```

### Note:
When you first time copy-paste a command on VSCode IDE, your browser may ask you to allow permission to see informaiton on clipboard. Please select **"Allow"**.

![allow-clipboard](/static/images/allow-clipboard.png)

- The output assumed-role name should look like the following:

![correct-iam-role](/static/images/correct-iam-role.png)

- Set the Amazon EKS cluster variables :

```bash
export CLUSTER_NAME=eksworkshop
```


- Check if region and cluster names are set correctly

```bash
echo $AWS_REGION
echo $CLUSTER_NAME
```

## Update the kube-config file:
Before you can start running all the Kubernetes commands included in this workshop, you need to update the kube-config file with the proper credentials to access the cluster. To do so, in your VSCode IDE terminal run the below command:

```bash
aws eks update-kubeconfig --name $CLUSTER_NAME --region $AWS_REGION
```


## Query the Amazon EKS cluster:
Run the command below to see the Kubernetes nodes currently provisioned:

```bash
kubectl get nodes
```

You should see two nodes provisioned (which are the on-demand nodes used by the Kubernetes controllers), such as the output below:


![get-nodes](/static/images/get-nodes.png)


You now have a VSCode IDE Server environment set-up ready to use your Amazon EKS Cluster! You may now proceed with the next step.

Now, you can go to next module **[Explore workshop environment](/030_module_explore_karpenter)** to continue with your workshop, once you are done and ready to clean up visit this page and execute commands in Part 4 Clean up below.

### Part 4 : Clean up

Delete Cloud formation stack to clean up, Please note this will take upto 30 mins. 

Note:  sometimes it fails to clean up due to VPC Dependency violations error due to ELB/EC2/ENI/Security groups/NAT gateway ..etc are blocking VPC deletion. You may have to take manual action to clean up. 

```bash
aws cloudformation delete-stack --stack-name ${STACK_NAME} --region $REGION
aws cloudformation wait stack-delete-complete --stack-name ${STACK_NAME} --region $REGION
```


