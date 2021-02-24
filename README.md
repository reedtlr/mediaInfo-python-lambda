# mediaInfo-python-lambda

An AWS Lambda for retrieving metadata from video files when uploaded to an S3 bucket
## Table of contents
* [General info](#general-info)
* [Authors](#authors)
* [Technologies](#technologies)
* [Install](#install)
* [Testing](#testing)

## General info
This lambda utilizes the mediainfo library to retrieve metadata from video files (MPEG-4, AVI, MPEG-TS, MPEG-PS, FLV, H.264/AVC, DivX, H.263, H.265 ...) that are stored in a S3 bucket. The code is based in part on the tutorial and code from (https://github.com/kchokshifox/MediaEvaluator) and the mediainfo library from (https://mediaarea.net/en/MediaInfo) which is an opensource software under a BSD-style license "Copyright (c) 2002-2021 MediaArea.net SARL. All rights reserved." The desired meta data fields need to added to the response. For this example, wrapperType, codec, bitRate, width, height, and resolution have been included as they are commonly included in most video format metadata. 

## Authors

- [**Robert Cullen-Keel**](https://github.com/motoroboto)
- [**Dexter Garner**](https://github.com/johndexteriv)
- [**Reed Taylor**](https://github.com/reedtlr)
- [**Thomas Tutchings**](https://github.com/tutchings)
- [**Tom Nisbet**](https://github.com/TRNisbet)
- [**Bennett Gould**](https://github.com/bpgould)

	
## Technologies
Project is created with:
* Python
* AWS EC2 or Docker Amazon Linux image
* AWS CLI
* AWS S3
* AWS IAM roles
* AWS Lambda
* Docker

## Install
Open VSCode to add any aditional meta data fields that you need. Add your MongoDB Atlas db API to line 122. Be sure to create the collection first in MongoDB. Zip the folder and upload to an S3 bucket. Create a Lambda function with a runtime of python 3.7. Create an IAM role with full acccess permisisons to Lambda and full access permissions to S3. Do not add this Lambda to a VPC. Apply the IAM role you just created to the Lambda. Use the 'Upload from' button to upload the zip file with the S3 URI address for the zip file. Now, add a trigger to your Lambda function for the S3 bucket that the video files will be stored in. Upload a video to test functionality. 


## Testing
For testing purposes, I recommend replacing lines 90 - 92 in mediaEvaluator.py with the following, 
```   bucketName = event['bucketName']```
```   objectName = event['objectKey'] ```
Now, you can use the Lambda Test feature on the AWS Console by entering the folling as your test,
```{```
```  "bucketName": "<s3 video files bucket>",```
```  "objectKey": "<file name as stored in s3 bucket>"```
```}```  