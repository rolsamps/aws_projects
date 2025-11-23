
# Study Guide Generator With Bedrock
This project uses Amazon Bedrock's foundational models to generate test questions for AWS certification study guides. The system reads exam description and list of topics covered from text files stored in an S3 bucket, which the AI model uses to generate relevant content. The generated output is then delivered to users via email through AWS SNS. The project focuses on implementing core functionalities by applying several simplification strategies. The system uses minimal error handling to maintain simplicity. The AI model employs a basic prompt structure with the format "Create 3 quiz questions from this text: [content]" for content generation. All emails and S3 files use plain text formatting without HTML to reduce complexity. The S3 bucket name is hardcoded directly in the application code for straightforward implementation. Finally, CloudFormation handles all infrastructure deployment to ensure consistent resource provisioning.
### Key Resources
- **CloudFormation template**: 
  - A CloudFormation template is a JSON or YAML file that defines AWS resources and their configurations as code. You can deploy the templates using AWS CLI, CloudShell terminal, or the AWS Console.
  - Deploy a CloudFormation template using AWS CLI with the cloudformation create-stack command and the --stack-name, --template-body, and --capabilities parameters to specify deployment settings and resource configurations.
  - The stack name is a unique identifier that enables multiple template deployments with structured resource naming—the best practice for reusability, though keep in mind some resources have specific naming requirements.
- **Bedrock**: 
  - Amazon Bedrock provides access for high performing foundational AI models from leading companies through use of a unified API. You can evaulate between a large selection of models to find one that best suits your case. Moreover, you can customize models with your own data through fine-tuning and RAG techniques, and create agents that work with your enterprise systems and data sources.
  - Amazon Bedrock offers a large selecetion of foundation models, some including Anthropic Claude, Amazon Titan, Meta Llama, AI21 Labs Jurassic, and Cohere Command, each with distinct capabilities, pricing, and performance characteristics.
- **SNS Topic**:
  - Amazon SNS is a fully managed messaging service that enables you to send notifications from cloud applications to subscribers through multiple delivery methods. 
  - It uses a publish-subscribe model where publishers send messages to topics, and subscribers receive those messages via their chosen protocols including email, SMS, HTTP endpoints, Lambda functions, and SQS queues. 
  - SNS operates through a simple lifecycle: create a topic, add subscriptions to enable endpoint message delivery, publish messages to send notifications, and clean up by deleting subscriptions and topics when no longer needed.
### Services
This project will allowed me to develop skills in several AWS services including:
- **CloudFormation**:  Infrastructure as code service that Deploys AWS resources using code templates for consistent project setup.
- **Bedrock**: AI service providing access to foundation models that generate study questions from text using Claude models.
- **S3**: Object storage service that stores file that will be processed by the Lambda function.
- **Lambda**: Serverless compute service that runs the application code that connects file uploads, AI processing, and notifications.
- **SNS**: Messaging service that sends email notifications to users when study guides are generated and ready for review.
#### Visualization
![image](project-6.png)
### Capabilities
Working on this project provided an opportunity to gain experience in performing engineering tasks for the cloud and AI including the following skills:
- **Infrastructure Deployment**: Deploying AWS resources using CloudFormation templates.
- **AI Model Integration**: Generating Bedrock AI responses and evaluating AI model capabilities.
- **Storage Management**: Managing file storage and access permissions with S3.
- **Notification Setup**: Configuring email subscriptions for SNS topic messaging.
- **Serverless Development**: Building Lambda functions for cross-service integration.
- **Security Management**: Managing execution role permissions and IAM policies.
### Objectives
####  1. Create a CloudFormation template to deploy necessary resources.
####  2. Configure an SNS topic and include an email subscription.
####  3. Upload simple text files to S3 for basic prompts and study topics.
####  4. Subscribe email to the SNS topic for message delivery.
####  5. Test the Lambda function and execution role to confirm service integration.
####  6. Include the Bedrock response module in Lambda code.
####  7. Enable SNS to send email communications.
### Questions
#### What are the methods to deploy and update CloudFormation templates?
 - Validate operation: aws cloudformation validate-template --template-body file://template.yaml
 - Deploy operation: aws cloudformation create-stack --stack-name MyStack --template-body file://template.yaml --capabilities CAPABILITY_IAM
 - Update operation: aws cloudformation update-stack --stack-name MyStack --template-body file://template.yaml
####  What is the purpose of the outputs section being defined in the cloudformation template?
 - Outputs define values that are displayed after a CloudFormation stack is successfully created or updated. This is particularly important in some cases because AWS sometimes modifies resource names for uniqueness during deployment.
 - These outputs serve several key purposes: they return resource information by exporting important resource identifiers, ARNs, URLs, or other attributes that other stacks or users might need; they provide cross-stack references by allowing other CloudFormation stacks to import these values using the Fn::ImportValue function; and they provide stack information by giving users easy access to key resource details without having to look them up in the AWS console.
### References
- **AWS Cloudformation deployment best pratices**: https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/best-practices.html#parmconstraints
- **AWS Cloudformation template guide**: https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-guide.html
- **wWat is Bedrock?**: https://docs.aws.amazon.com/bedrock/latest/userguide/what-is-bedrock.html
- **Bedrock key terminology**: https://docs.aws.amazon.com/bedrock/latest/userguide/key-definitions.html
- **Bedrock supported models**: https://docs.aws.amazon.com/bedrock/latest/userguide/models-supported.html
- **AWS SNS Getting Started Guide**: https://docs.aws.amazon.com/sns/latest/gsg/getting-started.html
- **Bedrock model customization**: https://docs.aws.amazon.com/bedrock/latest/userguide/custom-models.html
### Other Options
#### As an example, comparing differences between Claude's latest Haiku and Sonnet offerings demonstrates the differences in their use cases:
- Claude Sonnet 4.5: Best suited for complex coding and autonomous tasks, offering extended autonomous operation for hours-long projects with advanced context management and tool coordination, providing the highest intelligence across most complex tasks.
- Claude Haiku 4.5: The fastest model with cost-effective performance, delivering near-frontier intelligence that matches Sonnet 4 performance while operating at 2x faster speed with optimized output and one-third the cost, making it ideal for high-volume deployments.
#### Expanding on the customization methods mentioned previously, this is a deeper look into how these techniques are implemented:
- Fine-tuning trains foundation models on custom datasets for specific tasks. Implementation involves uploading training data to S3 in JSONL format, creating a fine-tuning job via Bedrock console or API, monitoring training progress, and deploying the custom model to an endpoint. 
- RAG (Retrieval Augmented Generation) connects models to external knowledge bases for real-time information retrieval. Implementation includes creating a knowledge base in Bedrock with vector embeddings, uploading documents to S3 and syncing with the knowledge base, and using the RetrieveAndGenerate API for context-aware responses.
### Tips
- Cloudshell file Upload Methods: upload files directly on Cloudshell by clicking Actions and selecting "Upload file," create new files using text editors like nano or vim to copy template contents, or clone files from GitHub using git commands with the repository link.
- When configuring Lambda execution role permissions in the AWS Console, you may encounter IAM errors indicating unrecognized actions. These errors typically result from one of the following causes: preview services, custom services, services that don't support the visual editor, actions that don't support the visual editor, resource types that don't support the visual editor, or typos in action names.
