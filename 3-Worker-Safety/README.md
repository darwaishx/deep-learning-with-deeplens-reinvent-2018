# Build worker safety system using DeepLens and Amazon Rekognition

## Learning Objectives of This lab
In this lab your will the following:
- Create and deploy object detection project to DeepLens.
- Extend the DeepLens object detection project to detect persons and upload frames to S3.
- Create lambda function to identify persons who are not wearing safety hats.
- Analyze results using IoT and CloudWatch.

![](assets/worker-safety-arch.png)

### Create S3 Bucket

1. Go to S3 in AWS Console at https://s3.console.aws.amazon.com/s3/home?region=us-east-1.
2. Click on Create bucket.
3. Under Name and region:
  - Bucket name: Enter a bucket name your-user-id-worker-safety (example: user-01-01-worker-safety)
  - Choose US East (N. Virginia)
  - Click Next.
4. Leave default values for following steps and on last screen click Create bucket.

### Create Lambda Function

1. Go to Lambda in AWS Console at https://console.aws.amazon.com/lambda/home?region=us-east-1#/functions.
2. Click on Create function.
3. Under Create function, Author from scratch should be selected as default.
4. Under Author from scratch:
  - Name: your-user-id-worker-safety-cloud (example: user-01-01-worker-safety-cloud)
  - Runtime: Python 3.7
  - Role: Choose and existing role
  - Existing role: deeplens-workshop-cloud-lambda-role
  - Click Create function
5. Under Environment variables, add a variable:
  - Key: iot_topic
  - Value: your-username-01-01-worker-safety (example: user-01-01-worker-safety)
6. Download [lambda.zip](./code/lambda.zip).
7. Under Function code:
  - Code entry type: Upload a zip file
  - Under Function package, click Upload and select the zip file you downloaded in earlier step.
  - Click Save.
8. Under Add triggers, select S3.
9. Under Configure triggers:
  - Bucket: Select the S3 bucket you just created in earlier step.
  - Event type: Leave default Object Created (All)
  - Leave defaults for Prefix and Suffix and make sure Enable trigger checkbox is checked.
  - Click Add.
  - Click Save on the top right to save changed to Lambda function.

### Create DeepLens Inference Lambda Function

1. Go to Lambda in AWS Console at https://console.aws.amazon.com/lambda/home?region=us-east-1#/functions.
2. Click on Create function.
3. Under Create function, select Blueprints.
4. Under Blueprints, type greengrass and hit enter to filter blueprint templates.
5. Select greengrass-hello-world and click Configure.
6. Under Basic information:
  - Name: your-user-id-worker-safety-deeplens (example: user-01-01-worker-safety-deeplens)
  - Role: Choose and existing role
  - Existing role: deeplens-workshop-device-lambda-role
  - Click Create function.
7. Copy the code from [deeplens-lambda.py](./code/deeplens-lambda.py) and paste under Function code for the lambda function.
8. Go to line 34 and modify line below with the name of your S3 bucket created in the earlier step.
  - bucket_name = "REPLACE-WITH-NAME-OF-YOUR-S3-BUCKET"
9. Click Save.
10. Click on Actions, and then "Publish new version".
11. For Version description enter: Detect person and push frame to S3 bucket. and click Publish.

## Create DeepLens Project

1. Using your browser, open the AWS DeepLens console at https://console.aws.amazon.com/deeplens/.
2. Choose Projects, then choose Create new project.
3. On the Choose project type screen
  - Choose Create a new blank project, and click Next.
4. On the Specify project details screen
   - Under Project information section:
      - Project name: your-user-name-worker-safety (example: user-01-01-worker-safety)
   - Under Project content:
      - Click on Add model, click on radio button for deeplens-object-detection and click Add model.
      - Click on Add function, click on radio  button for your lambda function (example: user-01-01-worker-safety-deeplens) lambda function and click Add function.
  - Click Create. This returns you to the Projects screen.

## Deploy DeepLens Project

Next you will deploy the Object Detection project you just created.

1. From DeepLens console, On the Projects screen, choose the radio button to the left of your project name, then choose Deploy to device.
2. On the Target device screen, from the list of AWS DeepLens devices, choose the radio button to the left of the device where you want to deploy this project.
3. Choose Review.
   This will take you to the Review and deploy screen.

   If a project is already deployed to the device, you will see a warning message
   "There is an existing project on this device. Do you want to replace it?
   If you Deploy, AWS DeepLens will remove the current project before deploying the new project."

4. On the Review and deploy screen, review your project and click Deploy to deploy the project.
   This will take you to to device screen, which shows the progress of your project deployment.

## View Output of Your Project

1. Go to IoT Console at https://console.aws.amazon.com/iot/home?region=us-east-1#/test
2. Under Subscription topic enter your-username-01-01-worker-safety (example: user-01-01-worker-safety) and click Subscribe to topic.
3. You should now see JSON message with a list of people detected and whether they are wearing safety hats or not.
4. To view messages coming from DeepLens as it is processing frames, subscribe to topic for your DeepLens device.

## View Worker-Safety Alerts in CloudWatch Dashboard

- Go to CloudWatch Console at https://console.aws.amazon.com/cloudwatch/home?region=us-east-1#
- Create a dashboard called “worker-safety-dashboard-your-name”
- Choose Line in the widget
- Under Custom Namespaces, select “string”, “Metrics with no dimensions”, and then select PersonsWithSafetyHat and PersonsWithoutSafetyHat.
- Next, set “Auto-refresh” to the smallest interval possible (1h), and change the “Period” to whatever works best for you (1 second or 5 seconds)

NOTE: These metrics will only appear once they have been sent to Cloudwatch via the Rekognition Lambda. It may take some time for them to appear after your model is deployed and running locally.
