
    Resources:
      MyRole:
        Type: AWS::IAM::Role
        Properties:
          AssumeRolePolicyDocument:
            Version: "2012-10-17"
            Statement:
              -
                Effect: "Allow"
                Principal:
                  Service:
                    - "glue.amazonaws.com"
                Action:
                  - "sts:AssumeRole"
          Path: "/"
          ManagedPolicyArns:
            ['arn:aws:iam::aws:policy/service-role/AWSGlueServiceRole']
          Policies:
            -
              PolicyName: "S3BucketAccessPolicy"
              PolicyDocument:
                Version: "2012-10-17"
                Statement:
                  -
                    Effect: "Allow"
                    Action: 
                      - "s3:GetObject"
                      - "s3:PutObject"
                    Resource: 
                      !Join
                        - ''
                        - - arn:aws:s3:::demobucket01234567891
                          - "*"

      MyDatabase:
        Type: AWS::Glue::Database
        Properties:
          CatalogId: !Ref AWS::AccountId
          DatabaseInput:
            Name: "dbcrawler"
            Description: "TestDatabaseDescription"
            LocationUri: "TestLocationUri"

      MyCrawler:
        Type: AWS::Glue::Crawler
        Properties:
          Name: "testcrawler"
          Role: !GetAtt MyRole.Arn
          DatabaseName: !Ref MyDatabase
          Targets:
            S3Targets:
              - Path: s3://demobucket01234567891/read/FINAL450.xlsx
          SchemaChangePolicy:
            UpdateBehavior: "UPDATE_IN_DATABASE"
            DeleteBehavior: "LOG"
          Schedule:
            ScheduleExpression: "cron(0/10 * ? * MON-FRI *)"

      MyGlueJob:
        Type: AWS::Glue::Job
        Properties:
          Command:
            Name: pythonshell
            PythonVersion: 3
            ScriptLocation: "s3://cf-templates-14gnq2uqgi0gl-us-east-2/app.py"
          DefaultArguments:
            "--job-bookmark-option": "job-bookmark-enable"
          ExecutionProperty:
            MaxConcurrentRuns: 2
          #GlueVersion: Python 3.7
          #NumberOfWorkers: 2
          MaxRetries: 2
          Timeout: 10
          Name: cf-job1
          Role: !GetAtt MyRole.Arn
