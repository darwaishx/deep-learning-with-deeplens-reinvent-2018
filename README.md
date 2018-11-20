# Get Started with Deep Learning and Computer Vision Using AWS DeepLens

## Learning Objectives of This lab
In this lab your will the following:
- Create and deploy object detection project to DeepLens.
- Modify the DeepLens object detection inference lambda function to detect persons and upload frame to S3.
- Create lambda function to identify persons who are not wearing safety hats.
- Analyze results using IoT and CloudWatch.

## Architecture

![](3-Worker-Safety/assets/worker-safety-arch.png)

## Modules

This workshop is broken up into following modules:

1. [Create and Deploy DeepLens Project](1-DeepLens-Project)
2. [Object Detection using Amazon Sage Maker](2-Object-Detection)
3. [Worker Safety End to End Solution](3-Worker-Safety)


## Clean Up
Delete CloudFormatoin template and it will delete resources it created. Manually delete the S3 bucket as CloudFormation will not delete the bucket if it is not empty. Delete other resources created during the lab including SageMaker instance.
