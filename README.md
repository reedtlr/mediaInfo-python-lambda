# mediaInfo-python-lambda

An AWS Lambda for retrieving metadata from video files when uploaded to an S3 bucket
## Table of contents
* [General info](#general-info)
* [Authors](#authors)
* [Technologies](#technologies)
* [Install](#install)
* [Testing](#testing)

## General info
This lambda utilizes the mediainfo library to retrieve metadata from video files (MPEG-4, AVI, MPEG-TS, MPEG-PS, FLV, H.264/AVC, DivX, H.263, H.265 ...) that are stored in a S3 bucket. The code is based in part on the tutorial and code from (https://github.com/kchokshifox/MediaEvaluator) and the mediainfo library from (https://mediaarea.net/en/MediaInfo) which is an opensource software under a BSD-style license "Copyright (c) 2002-2021 MediaArea.net SARL. All rights reserved." The desired meta data fields need to be added to the response (lines 58-81). For this example, wrapperType, codec, bitRate, width, height, and resolution have been included as they are commonly included in most video format metadata. 

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
* AWS EC2 or Docker with Amazon Linux image
* AWS CLI
* AWS S3
* AWS IAM roles
* AWS Lambda

## Install
Open VSCode to add any aditional meta data fields that you need. Add your MongoDB Atlas db API to line 122. Be sure to create the collection first in MongoDB. Zip the folder and upload to an S3 bucket. Create a Lambda function with a runtime of python 3.7. Create an IAM role with full acccess permisisons to Lambda and full access permissions to S3. Do not add this Lambda to a VPC. The resource based policy in your Lambda should look like the following, 
```{
  "Version": "2012-10-17",
  "Id": "default",
  "Statement": [
    {
      "Sid": "lambda-de46be74-cbf3-4f18-87ca-484754dcaddd",
      "Effect": "Allow",
      "Principal": {
        "Service": "s3.amazonaws.com"
      },
      "Action": "lambda:InvokeFunction",
      "Resource": "arn:aws:lambda:us-east-1:616082320291:function:<lambda_name>",
      "Condition": {
        "StringEquals": {
          "AWS:SourceAccount": "<aws_account_number>"
        },
        "ArnLike": {
          "AWS:SourceArn": "<ARN_s3_video_bucket>"
        }
      }
    }
  ]
}
```

Apply the IAM role you just created to the Lambda. Add the following permissions to the S3 bucket holding your videos, 
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "AWS": "<full_ARN_for_the_medinainfo_lambda>"
            },
            "Action": [
                "s3:GetObject",
                "s3:GetObjectAcl"
            ],
            "Resource": "arn:aws:s3:::<bucket_name>/*"
        }
    ]
}
```

 Use the 'Upload from' button to upload the zip file with the S3 URI address. Now, add a trigger to your Lambda function for the S3 bucket that the video files will be stored in. Upload a video to test functionality. 


## Testing
For testing purposes, I recommend replacing lines 90 - 92 in mediaEvaluator.py with the following, 

```   
bucketName = event['bucketName']
objectName = event['objectKey']
```

Now, you can use the Lambda Test feature on the AWS Console by entering the folling as your test,

```
{
"bucketName": "<s3 video files bucket>",
"objectKey": "<file name as stored in s3 bucket>"
}
```  