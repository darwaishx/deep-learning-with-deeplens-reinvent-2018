# Object detection using Amazon SageMaker

In this lab your will learn how to train a model to detect objects in Amazon SageMaker.

### Create S3 Bucket

1. Go to S3 in AWS Console at https://s3.console.aws.amazon.com/s3/home?region=us-east-1.
2. Click on Create bucket.
3. Under Name and region:
  - Bucket name: Enter a bucket name
  - Choose US East (N. Virginia)
  - Click Next.
4. Leave default values for following steps and on last screen click Create bucket.

## Create Amazon SageMaker instance

1. Using your browser, open the Amazon SageMaker console at https://console.aws.amazon.com/sagemaker.
2. Click on Create notebook instance
3. On the Create notebook instance  screen
  - Enter a name for notebook instance name.
  - Leave all other default options.
  - Scroll to the bottom of the screen, then click create notebook instance.

## Open and run object detection notebook

1. After SageMaker is ready, click Open to go the Jupyter UI.
2. Navigate to SageMaker Examples tab.
3. Expand Introduction to Amazon SageMaker Algorithms.
4. For "object_detection_image_json_format.ipynb" click Use and then click on Create Copy.
5. Under Setup, in second cell update bucket = '' with the  name of your S3 bucket you created earlier.
6. Under Training, in second cell with code change epochs=30 to epochs=1
6. Run the notebook by following through each cell. To run, you can either use the play button in the toolbar or use keyboard shortcut: ctrl + Enter (windows) or cmd + Enter (Mac).
