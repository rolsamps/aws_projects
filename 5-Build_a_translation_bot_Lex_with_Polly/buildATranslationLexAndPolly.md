# Build a Translation Bot using Lex with Polly
This project creates a language translation application with integrated text-to-speech capabilities using Amazon Polly. The app features multiple voice options for different languages and provides a conversational interface through Amazon Lex. A Lambda function orchestrates all services, and the application is deployed to Slack via API endpoints. This project serves as an excellent way to develop skills with the Polly service and learn service integration patterns using Lambda.
### Key Resources
- **AWS Lambda Function**:
  - Apply dictionary operations for language-to-voice mapping when requesting speech synthesis response, using slot value extraction to detect desired language and text to translate.
  - Exception handling with try/except blocks are used for AWS service calls. Including the response and error handling.
  - Translation Module for source language detection, text translation with auto-detection, and target language mapping.
- **Amazon Lex**:
  - The Slot Elicitation step occurs after the Initial Response where the bot will enter the ElicitIntent dialog state. Configured for sequential prompting with language first, then text. 
  - Fulfillment processing of user request with Lambda function integration. The function executes the business logic after collecting the required slot values and returns the final response.
  - Multi-format responses with different response types including PlainText, SSML, and ImageResponseCard.
- **Amazon Polly**:
  - S3 Bucket is used as an audio file repository with public read access configuration. 
  - Can manage access control using presigned URL generation for temporary file access which is used to upload objects to your Amazon S3 bucket.
- **API Deployment**:
  - Connection with OAuth flow allows channel integration with Slack workspace. configured with platform-specific message formatting and delivery.
  - API Endpoints created using Webhook URLs to enable external platform communication.
### Services
This project will allowed me to develop skills in several AWS services including:
- **Translate**: Auto-detect source language and translate to target language.
- **Comprehend**: Natural language processing service for sentiment analysis and language detection.
- **Lambda**: Serverless function orchestration managing Lex integration and translation workflows.
- **Lex**: Conversational interface with slot and intent handling incuding dialog state management for sequential user prompting.
- **Cloudwatch**: Monitoring and logging service for tracking translation requests.
- **S3**: Audio file storage repository and presigned URL generation for secure temporary access and cross-platform audio delivery.
- **Polly**: Text-to-speech synthesis converting text to audio with language-specific voices.
#### Visualization
![image](project-5.png)
### Capabilities
Working on this project provided an opportunity to gain experience in performing engineering tasks for the cloud building upon previous Lex and Lambda experience while introducing audio generation with Amazon Polly and other concepts:
- **Multi-Service Integration**: Orchestrating Translate, Polly, S3, and Lex services.
- **Conversational AI Development**: Building natural language interfaces using Amazon Lex with slot elicitation.
- **Testing & Validation**: Lambda function and Lex bot testing and refinement.
- **Error Handling & Resilience**: Implementing robust exception handling for AWS service failures.
- **Audio Processing**: Managing asynchronous Polly tasks and S3 file operations.
- **Security Configuration**: IAM permissions and S3 access control implementation.
- **Platform Integration**: Slack API deployment with OAuth authentication and webhook configuration.
### Objectives
#### 1. Create a Lambda Function for language detection and translate operation
#### 2. Implement audio speech translation using Amazon Polly with language-appropriate voices
#### 3. Create an Amazon Lex Bot and configure chatbot interface
#### 4. Configure Lambda Function Code to inegrate Backend with the Frontend
#### 5. Set Up IAM Permissions
#### 6. Test and Refine Lex bot and lambda function code
#### 7. Deploy the Bot with Slack for real-world messaging platform
### Questions
## What is Amazon Polly and how does it work to create lifelike speech?
Amazon Polly supports multiple languages and includes a variety of lifelike voices which you can use to create apps with synthesized speech to increase engagement and availability for users. It uses Neural TTS to analyze text and generate context and pronunciation with many voice options available. Poly integrates with Lambda through the boto3 client and uses these 3 key operations: text-to-speech synthesis, voice selections, and audio storage.
## What strategies can be used to handle long Polly synthesis tasks?
Amazon Polly provides long-form TTS for texts over 3,000 characters using asynchronous synthesis tasks that can handle up to 200,000 characters. With Asynchronous synthesis tasks, you can implement polling mechanism with timeout and status checking for better optimization with the execution of the task and error handling. 
## What is a dialog action?
The interaction from backend to frontend is handled through specific states called Dialog Actions. These define the next action that the bot must perform in its interaction with the user. The dialog flow follows three main states: Delegate (assigns Amazon Lex to determine the next step automatically), ElicitSlot (prompts the user to provide a specific slot value), and ElicitIntent (prompts the user to express or clarify their intent).
## How to make audio files accessible to users?
Use S3 presigned URLs with appropriate expiration times, presigned URLs provide temporary access to audio files.
## Can you move an s3 bucket to another region without having to recreate it?
No, S3 buckets cannot be moved to another region without recreating them. S3 buckets are permanently bound to their creation region due to three key constraints: buckets are region-specific resources that remain in their original location, bucket names must be globally unique across all regions, and each AWS region maintains separate S3 infrastructure that cannot be transferred.
### References
- **Integrating an AWS Lambda function into your bot?**:
- **Lambda with lex integration?**: https://docs.aws.amazon.com/lexv2/latest/dg/lambda.html
- **Languages-to-text mapping, speech synthesis?**: https://docs.aws.amazon.com/polly/latest/dg/synthesize-example.html
- **How to implement poly using boto3, aws service calls?**: https://boto3.amazonaws.com/v1/documentation/api/latest/index.html
- **Slot Elicitation**: https://docs.aws.amazon.com/lexv2/latest/dg/paths-nextstep.html
- **Lex Fullfillment**: https://docs.aws.amazon.com/lexv2/latest/dg/what-is.html
- **S3 Presigned URL**: https://docs.aws.amazon.com/AmazonS3/latest/userguide/PresignedUrlUploadObject.html
- **Lex API Integration**: https://docs.aws.amazon.com/lexv2/latest/dg/deploying-messaging-platform.html
- **What is Amazon Polly?**: https://docs.aws.amazon.com/polly/latest/dg/what-is.html
- **Long Synthesis**: https://docs.aws.amazon.com/polly/latest/dg/asynchronous.html
- **Lambda Response Format**: https://docs.aws.amazon.com/lexv2/latest/dg/lambda-response-format.html
- **S3 Bucket Regions and Locations**: https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingBucket.html
- **Response Cards**: https://docs.aws.amazon.com/lexv2/latest/dg/what-is.html
### Other Options
#### Other ways to deploy the app include the following:
- Integrating Amazon Lex bots with messaging platforms (Facebook, Slack, MS Teams, Twilio, Discord) and using Java applications to interact with Amazon Lex bots through AWS SDK.
- Can be integrated with contact centers (Amazon Chime SDK, Amazon Connect, Genesys Cloud) for customer service automation and deployed through custom web UI interfaces for browser-based chatbot experiences.
#### Response Cards are used to provide interactive UI elements in Amazon Lex that display rich content beyond plain text responses. Can be used with displaying audio content generated by polly.
### Tips
- When deploying, test bot in actual channels (Slack, MS teams) rather than just Lex console for full functionality 