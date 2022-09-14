# Sample Python Code 
### lambda_function.py
      import json
    from datetime import datetime

    now = datetime.now()
    current_time = now.strftime("%H:%M:%S")

    def lambda_handler(event, context):

        print("Hello Vikash")
        print("Current time is:",current_time)

        return{
            'statusCode': 200,
            'body': json.dumps('Hello from lambda !!')
        }


### Pass Values to the function call 

   lambda_handler('s3','context')
   

![image](https://user-images.githubusercontent.com/35370115/190150239-b03ff26f-8fd9-4baf-8be5-a1812ad56e67.png)


### lambda_function.yml

            AWSTemplateFormatVersion : 2010-09-09
            Description: Example Stack



            #Creating the mycloudbucket19998
            Parameters:
              BucketName:
                 Type: String
                 Default: mycloudbucket19998

            Resources:
             # Applying trigger on the mycloudbucket19998, whenever we'll uload .txt file function will be triggered
              Bucket:
               Type: AWS::S3::Bucket
               Properties:
                 BucketName: !Ref BucketName

                 NotificationConfiguration:
                   LambdaConfigurations:
                     - Event: 's3:ObjectCreated:*'
                       Filter:
                        S3Key:
                          Rules:
                          - Name: suffix
                            Value: .txt
                       Function: !GetAtt Lambda.Arn

              Lambda:
                Type: AWS::Lambda::Function
                Properties:
                  FunctionName: "myfirstlambdafunction"
                  Handler: lambda_function.lambda_handler
                  Role: arn:aws:iam::373407815244:role/service-role/TransactionProcessor-role-9iv7s0g0
                  Code:
                    S3Bucket: demobucket01234567891
                    S3Key: code/myfirstlambdafunction.zip
                  Runtime: "python3.8"
                  Timeout: 300
                  MemorySize: 512
                  TracingConfig:
                    Mode: Active

              S3InvokeLambdaPermission:
                Type: AWS::Lambda::Permission
                Properties:
                  Action: lambda:InvokeFunction
                  FunctionName: !Ref Lambda
                  Principal: s3.amazonaws.com
                  SourceArn: !Sub arn:aws:s3:::${BucketName}

![image](https://user-images.githubusercontent.com/35370115/190159235-d2810ff3-8872-4036-9cc7-253dcb562104.png)
