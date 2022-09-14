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
