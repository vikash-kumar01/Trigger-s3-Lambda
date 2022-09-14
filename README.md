# Sample Python Code 

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
