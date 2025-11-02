# Build a Language Translation Bot Using Lex with Polly
This project creates a language translation application with integrated text-to-speech capabilities using Amazon Polly. The app features multiple voice options for different languages and provides a conversational interface through Amazon Lex. A Lambda function orchestrates all services, and the application is deployed to Slack via API endpoints. This project serves as an excellent way to develop skills with the Polly service and learn service integration patterns using Lambda.
# Key Resources
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
#### Visualization
![image](project-5.png)
# Capabilities
Working on this project provided an opportunity to gain experience in performing engineering tasks for the cloud building upon previous Lex and Lambda experience while introducing audio generation with Amazon Polly and other concepts:
- **Multi-Service Integration**: Orchestrating Translate, Polly, S3, and Lex services.
- **Conversational AI Development**: Building natural language interfaces using Amazon Lex with slot elicitation.
- **Testing & Validation**: Lambda function and Lex bot testing and refinement.
- **Error Handling & Resilience**: Implementing robust exception handling for AWS service failures.
- **Audio Processing**: Managing asynchronous Polly tasks and S3 file operations.
- **Security Configuration**: IAM permissions and S3 access control implementation.
- **Platform Integration**: Slack API deployment with OAuth authentication and webhook configuration.
# Services
This project will allowed me to develop skills in several AWS services including:
- **Translate**: Auto-detect source language and translate to target language.
- **Comprehend**: Natural language processing service for sentiment analysis and language detection.
- **Lambda**: Serverless function orchestration managing Lex integration and translation workflows.
- **Lex**: Conversational interface with slot and intent handling incuding dialog state management for sequential user prompting.
- **Cloudwatch**: Monitoring and logging service for tracking translation requests.
- **S3**: Audio file storage repository and presigned URL generation for secure temporary access and cross-platform audio delivery.
- **Polly**: Text-to-speech synthesis converting text to audio with language-specific voices.
# Objectives
#### 1. Create a Lambda Function for language detection and translate operation
#### 2. Implement audio speech translation using Amazon Polly with language-appropriate voices
#### 3. Create an Amazon Lex Bot and configure chatbot interface
#### 4. Configure Lambda Function Code to inegrate Backend with the Frontend
#### 5. Set Up IAM Permissions
#### 6. Test and Refine Lex bot and lambda function code
#### 7. Deploy the Bot with Slack for real-world messaging platform
# Questions
## What is Amazon Polly and how does it work to create lifelike speech?
Amazon Polly supports multiple languages and includes a variety of lifelike voices which you can use to create apps with synthesized speech to increase engagement and availability for users. It uses Neural TTS to analyze text and generate context and pronunciation with many voice options available. Poly integrates with Lambda through the boto3 client and uses these 3 key operations: text-to-speech synthesis, voice selections, and audio storage.
## What strategies can be used to handle long Polly synthesis tasks?
Amazon Polly provides long-form TTS for texts over 3,000 characters using asynchronous synthesis tasks that can handle up to 200,000 characters. With Asynchronous synthesis tasks, you can implement polling mechanism with timeout and status checking for better optimization with the execution of the task and error handling. 
## What is a dialog action?
The interaction from backend to frontend is handled through specific states called Dialog Actions. These define the next action that the bot must perform in its interaction with the user. The dialog flow follows three main states: Delegate (assigns Amazon Lex to determine the next step automatically), ElicitSlot (prompts the user to provide a specific slot value), and ElicitIntent (prompts the user to express or clarify their intent).
## How to make audio files accessible to users?
Use S3 presigned URLs with appropriate expiration times, presigned URLs provide temporary access to audio files.
## Can you move an s3 bucket to anoter region without having to recreate it?
No, S3 buckets cannot be moved to another region without recreating them. S3 buckets are permanently bound to their creation region due to three key constraints: buckets are region-specific resources that remain in their original location, bucket names must be globally unique across all regions, and each AWS region maintains separate S3 infrastructure that cannot be transferred.
# References
lambda with lex integration: https://docs.aws.amazon.com/lexv2/latest/dg/lambda.html
languages-to-text mapping, speech synthesis: https://docs.aws.amazon.com/polly/latest/dg/synthesize-example.html
how to implement poly using boto3, aws service calls: https://boto3.amazonaws.com/v1/documentation/api/latest/index.html
Slot Elicitation: https://docs.aws.amazon.com/lexv2/latest/dg/paths-nextstep.html
Lex Fullfillment: https://docs.aws.amazon.com/lexv2/latest/dg/what-is.html
S3 Presigned URL: https://docs.aws.amazon.com/AmazonS3/latest/userguide/PresignedUrlUploadObject.html
Lex API Integration: https://docs.aws.amazon.com/lexv2/latest/dg/deploying-messaging-platform.html
What is Amazon Polly?: https://docs.aws.amazon.com/polly/latest/dg/what-is.html
Long Synthesis: https://docs.aws.amazon.com/polly/latest/dg/asynchronous.html
Lambda Response Format: https://docs.aws.amazon.com/lexv2/latest/dg/lambda-response-format.html
S3 Bucket Regions and Locations: https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingBucket.html
Response Cards: https://docs.aws.amazon.com/lexv2/latest/dg/what-is.html
# Other Options
-Other ways to deploy app:
Other ways to deploy the app include integrating Amazon Lex bots with messaging platforms (Facebook, Slack, MS Teams, Twilio, Discord) and using Java applications to interact with Amazon Lex bots through AWS SDK.
Additionally, bots can be integrated with contact centers (Amazon Chime SDK, Amazon Connect, Genesys Cloud) for customer service automation and deployed through custom web UI interfaces for browser-based chatbot experiences.
-Response Cards are used to provide interactive UI elements in Amazon Lex that display rich content beyond plain text responses. Can be used with displaying audio content generated by polly.
# Tips
-When deploying, test bot in actual channels (Slack, MS teams) rather than just Lex console for full functionality






















______


is Added missing Lex V2 response functions: delegate, elicit_slot, elicit_intent a requirment?

    Minimally, the Lambda response must include a sessionState object. Within that, provide a dialogAction object and specify the type field. Depending on the type of dialogAction that you provide, there may be other required fields for the Lambda response. These requirements are described as follows, alongside minimal working examples:
    Delegate lets Amazon Lex V2 determine the next step. No other fields are required.
    ElicitIntent prompts the customer to express an intent. You must include at least one message in the messages field to prompt elicitation of an intent.
    ElicitSlot prompts the customer to provide a slot value. You must include the name of the slot in the slotToElicit field in the dialogAction object. You must also include the name of the intent in the sessionState object.
    ConfirmIntent confirms the customer's slot values and whether the intent is ready to be fulfilled. You must include the name of the intent in the sessionState object and the slots to be confirmed. You must also include at least one message in the messages field to ask the user for confirmation of the slot values. Your message should prompt a "yes" or "no" response. If the user responds "yes", Amazon Lex V2 sets the confirmationState of the intent to Confirmed. If the user responds "no", Amazon Lex V2 sets the confirmationState of the intent to Denied.
    Close ends the fulfillment process of the intent and indicates that no further responses are expected from the user. You must include the name and state of the intent in the sessionState object. The compatible intent states are Failed, Fulfilled, and InProgress.
https://docs.aws.amazon.com/lexv2/latest/dg/lambda-response-format.html



have lambda let lex handle slot prompting for text_to_translate vs coding in slot handling on lambda function







______













AWS Lex Language Translation Bot with Polly Audio
A conversational AI bot that translates text between languages and provides audio pronunciation using Amazon Translate, Polly, and Lex with Japanese character conversion support via jaconv library.

Key Resources
Lambda Function:
Imports: boto3, time, botocore.exceptions, jaconv (deployment package)
Functions: TranslateText(), get_target_voice(), start_taskID(), get_speech_synthesis(), create_presigned_url()
Dictionary operations for language-to-voice mapping, slot value extraction
Exception handling with try/except blocks for AWS service calls
F-string formatting for dynamic responses and logging
Key Python Concepts: Functions and parameters, Dictionaries and nested data structures, Error handling with try/except, String manipulation and formatting, Object-oriented programming (method calls), External library usage (boto3, jaconv)
-
Lex Bot:
Intent: intent0 (translation request handler)
Slots:
language (target language for translation)
text_to_translate (source text input)

-
S3 Integration:
Polly audio file storage with presigned URL generation
Bucket permissions for Lambda read/write access




Objectives
1. Create a functional translation bot that accepts text input and target language
2. 
3. Handle Japanese character conversion using jaconv library via deployment package
4. 

Questions
How to handle slot elicitation order in Lex?
Force sequential slot filling by explicitly eliciting language first, then text
How to include external libraries in Lambda?
Create deployment package with jaconv library and upload as ZIP file


References
AWS Lex V2 Documentation: https://docs.aws.amazon.com/lexv2/latest/dg/
Slack Integration Guide: https://docs.aws.amazon.com/lexv2/latest/dg/deploy-slack.html
Lambda Deployment Packages: https://docs.aws.amazon.com/lambda/latest/dg/python-package.html

Other Options
Alternative Audio Delivery: Use SSML audio tags for direct playback in voice channels
URL Shortening: Implement custom URL shortener for cleaner user experience

Tips
Always validate S3 bucket accessibility before starting Polly tasks
Use session attributes to track slot elicitation state and store temporary data
Implement circuit breaker pattern for handling Polly service failures












Key Resources
Lambda Function Architecture:
Core Libraries: AWS SDK (boto3), time management, error handling (botocore.exceptions), external dependencies via deployment packages

Audio Processing Module: Text-to-speech synthesis, asynchronous task management, voice selection by language
Storage Integration: S3 file operations, presigned URL generation with expiration controls
Lex Integration Functions: Event parsing, slot value extraction, session state management, response formatting
Helper Utilities: Language-to-voice mapping dictionaries, error handling wrappers, logging mechanisms
Key Python Concepts: Asynchronous operations, nested data structures, exception chaining, string interpolation, object method chaining, external API integration

Lex Bot Configuration:
Intent Structure: Single primary intent for translation requests with fulfillment hooks
Slot Management:
Language slot (target language selection with built-in language types)
Text input slot (free-form text capture with validation)

Session Handling: Attribute persistence across conversation turns

Storage Infrastructure:

File Management: Automated cleanup policies and naming conventions
Security: Bucket policies restricting access to Lambda execution role

Integration Components:











Capabilities
Working on this project provided an opportunity to gain experience in performing engineering tasks for the cloud including:

Amazon Lex Service Mastery: Developed deep understanding of conversational AI architecture through hands-on implementation of intent management, slot elicitation patterns, and dialog flow control. Learned to configure bot aliases, manage session state persistence, and implement custom fulfillment logic that bridges natural language processing with backend services.

Lambda-Lex Integration Expertise: Gained comprehensive experience in building serverless fulfillment functions that parse Lex event structures, extract slot values from nested JSON, manage conversation context through session attributes, and return properly formatted responses. Mastered the intricacies of dialog actions (ElicitSlot, ElicitIntent, Delegate) and learned when to use each pattern for optimal user experience.

Translation Service Orchestration: Developed proficiency in integrating Amazon Translate with conversational interfaces, implementing auto-language detection workflows, and managing translation requests within the constraints of chat-based interactions. Learned to handle edge cases like unsupported language pairs and translation failures gracefully.

Robust Error Handling Implementation: Built comprehensive error handling strategies through iterative troubleshooting of real-world failures including:
Service Integration Errors: Implemented try-catch blocks with specific exception handling for ClientError, AccessDenied, and TaskNotFound scenarios
Slot Validation Issues: Debugged and resolved slot name mismatches between Lex configuration and Lambda code, learning the importance of consistent naming conventions
Asynchronous Task Management: Developed polling mechanisms with timeout handling for Polly synthesis tasks, implementing exponential backoff and circuit breaker patterns
IAM Permission Troubleshooting: Worked through complex permission issues involving cross-service access between Lambda, S3, Polly, and Lex, gaining deep understanding of resource-based policies

Multi-Service Architecture Design: Learned to orchestrate multiple AWS services (Lex, Lambda, Translate, Polly, S3) in a cohesive workflow, understanding service limits, latency considerations, and failure modes. Developed skills in managing asynchronous operations and state transitions across service boundaries.

Real-World Deployment Experience: Gained practical experience deploying conversational AI to production messaging platforms (Slack), including OAuth configuration, webhook management, and platform-specific response formatting. Learned the differences between testing in Lex console versus real-world channel integrations.

Advanced Lambda Development: Mastered deployment package creation for external dependencies, learned to optimize function performance for conversational latency requirements, and implemented comprehensive logging strategies for production debugging and monitoring.

































#########################################################################################################################


















































## list of topics from document
- How to use Artificial Intelligence service to detect the dominant language in texts.(amazon comprehend)
How to use Artificial Intelligence service to translate text. (amazon translate)
How to use Artificial Intelligence service to convert text into lifelike speech. (amazon polly)
How to create a Artificial Intelligence conversational interfaces bot to handle translation requests. (amazon lex)
-In this tutorial you are going to create a translator chatbot app, with Amazon Lex that will handle the frontend interaction with the user, and the backend will be in charge of an AWS Lambda Function with the AWS SDK for Python library Boto3 code using the following AWS services:
    Amazon Comprehend in charge of detecting the language entered.
    Amazon Translate it will translate into the desired language.
    Amazon Polly it delivers the audio with the correct pronunciation.
-Build It in Seven Parts
-
Part 1 - Create the Function That Detects the Language and Translates It Into the Desired Languag 🌎.
-Amazon Translate to translate across common languages unstructured text (UTF-8) documents or to build applications that work in multiple languagues using TranslateText from Boto3 Translate client, and Amazon Comprehend to detect the dominant language of the text you want to translate using the API DetectDominantLanguage from Boto3 Comprehend client
-determine the source language
-making an internal call to Amazon Comprehend
-Parameters Requires for TranslateText API: (listed in document)
*def TranslateText (text,language):
-
Part 2 - Create the Function to Converts Text Into Lifelike Speech 🦜.
-To create the Text to Speech function you are going to use Amazon Polly, an AI service that uses advanced deep learning technologies to synthes
-In this part you'll use the Boto3 Polly client to call the APIs StartSpeechSynthesisTask, and GetSpeechSynthesisTask to retrieve information of SpeechSynthesisTask based on its TaskID. It delivers status and a link to the Amazon S3 Bucket containing the output of the task.
-Parameters for Amazon Lex APIs (listed in document)
--Polly supports multiple languages and voices that allow synthesized speech #elaborate
*def get_target_voice(language):
*def start_taskID(target_voice,bucket_name,text):
*def get_speech_synthesis(task_id):
*def create_presigned_url(bucket_name, object_name, expiration=3600):
-
Part 3 - Configure the Chatbot Interface With Amazon Lex🤖.
-Amazon Lex is an AWS service that allows developers to build conversational interfaces for applications using voice and text.
-It provides the deep functionality and flexibility of natural language understanding (NLU) and automatic speech recognition (ASR)
-and simplifies building natural conversation experiences for applications without needing specialized AI/ML skills. It can also be integrate to mobile, web, contact center, messaging platform and other AWS services like AWS Lambda functios.
-Lex, has the following components:(listed in document)
-follow steps to create an amazon lex bot (listed in document)
-create slot type with the following parameters (listed in document)
-configure the intent with the following parameters (listed in document)
-This bot will invoke a Lambda function as a dialog code hook, to validate user input and to fulfill the intent.
-
Part 4 - Build the Interface Between the Backend and the Frontend.
-The interaction from backend to frontend will be handled through specific states called Dialog Action. It refers to the next action that the bot must perform in its interaction with the user. Possible values are:
    ConfirmIntent - The next action is asking the user if the intent is complete and ready to be fulfilled. This is a yes/no question such as "Place the order?"
    Close - Indicates there will not be a response from the user. For example, the statement "Your order has been placed" does not require a response.
    Delegate - The next action is determined by Amazon Lex.
    ElicitIntent - The next action is to determine the intent that the user wants to fulfill.
    ElicitSlot - The next action is to elicit a slot value from the user.
-The Conversation Flow’s Three Main States
    1. Delegate: The user starts the Intent, triggering the Lambda Function which is always listening. Lambda does not receive the expected language value, so it will Delegate Lex to continue handling the conversation by eliciting the language slot.
    2. Elicitslot: The user provides the language and Lex interprets the value as a language code. The Lambda Function sees the language value and asks Lex to ElicitSlot text_to_translate.
    3. ElicitIntent: The user provides the text to translate. Since the text is variable and unpredictable, Lex cannot interpret it as the text_to_translate value. So the Lambda Function interprets the text insted Lex and starts the translation and text-to-speech process. Finally, it replies to Lex with an ElicitIntent containing the translated text and a pre-signed link.
-Lambda Function needs to interpret the format of the Lex output events, which get passed to the Lambda function as input events
https://docs.aws.amazon.com/lexv2/latest/dg/lambda-input-format.html
-Lambda Function must extract the values in interpretations
https://docs.aws.amazon.com/lexv2/latest/APIReference/API_runtime_Interpretation.html
*def get_intent(intent_request):
*def get_slot(slotname, intent, **kwargs):
-To maintain the dialogue between the Lambda Function and Lex it is necessary to know the context that a user is using in a session (activeContexts) inside of the state of the user's session (sessionState) value. 
https://docs.aws.amazon.com/lexv2/latest/APIReference/API_runtime_ActiveContext.html
https://docs.aws.amazon.com/lexv2/latest/APIReference/API_runtime_SessionState.html
*def get_active_contexts(event):
*def get_session_attributes(event):
-To send the DialogAction state use this function definition:
*missing
-
Part 5 - Integrate the Backend With the Frontend.
With all the code built up in the previous parts, you are going to assemble the Lambda Handler as follows.
*def lambda_handler(event, context):
-Important: Import the necessary libraries, define the name of the bucket_name and initialize the clients for Boto3 from the AWS services, and Deploy to save. #repeat. import libraries, define bucket_name, initialize clients for Boto3
-Lambda Function Permissions
                "polly:SynthesizeSpeech",
				"polly:StartSpeechSynthesisTask",
                "polly:GetSpeechSynthesisTask",
				"comprehend:DetectDominantLanguage",
				"translate:TranslateText"
                -
                "s3:PutObject",
				"s3:GetObject"
-plus, some additional permissions
-
Part 6 - Let’s Get It to Work!
-Attaching a Lambda Function to a Bot Alias. Choose the name of the Lambda function to use.
-test the bot
-
Part 7 - Deploy Your Translator App.
-You need to create immutable versions in order to bring your bot into production.
-An alias is a pointer to a specific version of a bot. With an alias, you can easily update the version that your client applications are using without having to change any code. Aliases allow you to seamlessly direct traffic to different versions as needed.
-You already have everything you need to integrate your bot with messaging platforms, mobile apps, and websites, build your application by following some of these instructions:
    Using a Java application to interact with an Amazon Lex bot.
    Integrating an Amazon Lex bot with a messaging platform (Facebook, Slack, Twilio).
    Integrating an Amazon Lex bot with a contact center (Amazon Chime SDK, Amazon Connect, Genesys Cloud).
    Web UI for Your Chatbot.

___________________


6286 down

# structure of code
# import boto, def TranslateText from part 1
# import boto, import time, def get_target_voice, def start_taskID, def get_speech_synthesis, def create_presigned_url from part 2
# def get_intent, def get_slot, def get_active_contexts, def get_session_attributes from part 4
# def lambda_handler from part 5

-Create Slot Types
    In the left menu, choose Slot types, then Add slot type and select Add blank slot type.
    Complete with the following parameters for each slot type:

# useful reference links
-testing amazon lex bot
https://docs.aws.amazon.com/lexv2/latest/dg/test-bot.html
-amazon lex dialog action
https://docs.aws.amazon.com/lex/latest/dg/API_runtime_DialogAction.html
-amazon lambda input event and response format
https://docs.aws.amazon.com/lex/latest/dg/API_runtime_DialogAction.html

-created inline custom policy for lambda function
-needed to updated custom policy with s3 bucket name
-needed to create. bucket with amazon polly saved to it

# can you move an s3 bucket to another region without having to recreate it?
No, you cannot move an S3 bucket to another region without recreating it. S3 buckets are region-specific 
Why Buckets Can't Be Moved:
S3 buckets are region-bound - They're created in a specific region and stay there
Bucket names are globally unique - The same name can't exist in multiple regions
Regional infrastructure - Each region has its own S3 infrastructure

-You don't have the IAM permissions necessary to perform this operation (polly:StartSpeechSynthesisTask)
added polly fullaccess

##log notes
*missing from part 5 🚨Important: Import the necessary libraries, define the name of the bucket_name and initialize the clients for Boto3 from the AWS services, and Deploy to save.
-added import boto3 library
-defined bucket name variable with value as name of bucket
*No, the response functions delegate, elicit_slot, and elicit_intent are not included in boto3. These are custom helper functions you need to implement for Amazon Lex V2 response formatting.
-create alias, test bot, go from there
# corrections/ missing code
def delegate(active_contexts, session_attributes, intent):
    return {
        'sessionState': {
            'dialogAction': {'type': 'Delegate'},
            'intent': intent,
            'activeContexts': active_contexts,
            'sessionAttributes': session_attributes
        }
    }

def elicit_slot(slot_name, active_contexts, session_attributes, intent, messages):
    session_attributes["previous_slot_to_elicit"] = slot_name
    return {
        'sessionState': {
            'dialogAction': {'type': 'ElicitSlot', 'slotToElicit': slot_name},
            'intent': intent,
            'activeContexts': active_contexts,
            'sessionAttributes': session_attributes
        },
        'messages': messages
    }

def elicit_intent(active_contexts, session_attributes, intent, messages):
    return {
        'sessionState': {
            'dialogAction': {'type': 'ElicitIntent'},
            'activeContexts': active_contexts,
            'sessionAttributes': session_attributes
        },
        'messages': messages
    }

# Also fix line 175:
# Change from:
object_name,task_id = start_taskID(target_voice,bucket_name,text_ready)
# To:
task_id, object_name = start_taskID(target_voice,bucket_name,text_ready)

-line 6892
## full code corrected

Key corrections made:
Added missing Lex V2 response functions: delegate, elicit_slot, elicit_intent
Fixed variable order: task_id, object_name = start_taskID(...)
Added error handling in get_target_voice with .get(language, 'Amy')
Cleaned up formatting and removed unnecessary comments

-line 7088
-added debug code
-changed getsynthesis function, removed maxitem
-error regarding iam permissions
not authorized to perform: polly:DescribeVoices
-added permission polly:DescribeVoices, aws connection working
-sythenthis task stuck on timeout
-added error handling code of stuck Polly synthesis tasks

##log notes
-added s3 validation on start task function
-was able to work properly and save output to s3 bucket after 
policy update adding s3 permissions
-lambda function setting, extended duration timer to 15 sec

#remove default messages and delegate lex bot to handle slot prompting

##log notes 08/28
-need to setup to have lambda let lex handle slot prompting for text_to_translate
-simplify current lambda handler or revert back to code in tuturial
-reverter to tutorial code, missed try statement but readded it
-issue it regarding the intent portion with lex bot, lambda function is able to work when running test

Looking at your Lambda code, I can see the issue. The bot is eliciting the wrong slot because your slot validation logic has a flaw. Here's the problem and fix: 
Issue : When language is None, you're delegating back to Lex, but Lex might be trying to fill text_to_translate first instead of language.
Fix : Replace your current slot handling logic with this corrected version:
(more details in q prompt)

##lognotes0:08/29
The Fix
Your Lambda function is trying to elicit a slot called "text_to_translate" but your Lex bot only has a slot called "text".

-

To make the audio file show as a playable response in Lex bot, you need to use the SSML content type with an <audio> tag
lex bot test returning url still

Option 1: Use Response Cards (Recommended)

##lognotes0:08/30
-deployed app to slack
-added japanese voices for polly standard engine

##lognotes0:08/31

@@lognotes
@@lognotes
##lognotes:08/28
-need to setup to have lambda let lex handle slot prompting for text_to_translate
-simplify current lambda handler or revert back to code in tuturial
-reverter to tutorial code, missed try statement but readded it
-issue it regarding the intent portion with lex bot, lambda function is able to work when running test
##lognotes0:08/29
-recreated lambda slot handling logic due to error, having handler for language slot force step first instead of and before trying to fill text_to_translate slot
-need to test lambda function again
-some mozilla error popping up about blocking html iframe i dont know why, restart surface?
##lognotes0:08/30
-created new jason test code
-confirmed new test is working with update lambda slot handling logic
-tested lex bot again, still not working
-updated slot name from text to text_to_translate
-confirmed test is now working, it returns a presign url to s3 bucket where the audio file with the polly translation is stored
-how to make the audio file show in lex bot as a response?
-created message response in dictionary format
-tested adding reponse card to provide a button for the audio file polly translation response, added hashlib library to store url in 8-digit hash id, not working on lex bot chat reverted back
-error after reverting code, need to test if something missing
-fixed error, was syntax with extra bracket
-deploy web ui for chatbot using cloudformation? too much
##lognotes0:08/31
-created new slack channel
-deploying app api for lex bot to slack
-needed to activate public distribution
-skip cloudformation web ui for now, might revisit for future cloudformation project
-updated polly voice for japanese language for standard engine
-starting documentation



























######################################################################################

######################################################################################







draft<

# Build a Translation Bot with Lex and Polly
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
![image](.zero/labs/AWS-labs/5-Build_a_translation_bot_Lex_with_Polly/project-5.png)
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
## Can you move an s3 bucket to anoter region without having to recreate it?
No, S3 buckets cannot be moved to another region without recreating them. S3 buckets are permanently bound to their creation region due to three key constraints: buckets are region-specific resources that remain in their original location, bucket names must be globally unique across all regions, and each AWS region maintains separate S3 infrastructure that cannot be transferred.
### References
lambda with lex integration: https://docs.aws.amazon.com/lexv2/latest/dg/lambda.html
languages-to-text mapping, speech synthesis: https://docs.aws.amazon.com/polly/latest/dg/synthesize-example.html
how to implement poly using boto3, aws service calls: https://boto3.amazonaws.com/v1/documentation/api/latest/index.html
Slot Elicitation: https://docs.aws.amazon.com/lexv2/latest/dg/paths-nextstep.html
Lex Fullfillment: https://docs.aws.amazon.com/lexv2/latest/dg/what-is.html
S3 Presigned URL: https://docs.aws.amazon.com/AmazonS3/latest/userguide/PresignedUrlUploadObject.html
Lex API Integration: https://docs.aws.amazon.com/lexv2/latest/dg/deploying-messaging-platform.html
What is Amazon Polly?: https://docs.aws.amazon.com/polly/latest/dg/what-is.html
Long Synthesis: https://docs.aws.amazon.com/polly/latest/dg/asynchronous.html
Lambda Response Format: https://docs.aws.amazon.com/lexv2/latest/dg/lambda-response-format.html
S3 Bucket Regions and Locations: https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingBucket.html
Response Cards: https://docs.aws.amazon.com/lexv2/latest/dg/what-is.html
### Other Options
#### Other ways to deploy the app include the following:
- Integrating Amazon Lex bots with messaging platforms (Facebook, Slack, MS Teams, Twilio, Discord) and using Java applications to interact with Amazon Lex bots through AWS SDK.
- Can be integrated with contact centers (Amazon Chime SDK, Amazon Connect, Genesys Cloud) for customer service automation and deployed through custom web UI interfaces for browser-based chatbot experiences.
#### Response Cards are used to provide interactive UI elements in Amazon Lex that display rich content beyond plain text responses. Can be used with displaying audio content generated by polly.
### Tips
- When deploying, test bot in actual channels (Slack, MS teams) rather than just Lex console for full functionality

>








