# Event-Driven-Sales-DataPipeline
***
## Project Overview
This project is an overview of an Event Driven Sales Data Projection data pipeline that Process the Orders data based on their Status and route towards DynamoDB or SQS as per the Business requirement rules.
We will design a system using AWS services such as S3, Lambda, DynamoDB, Step Function and Event Bridge.

***

## Architectural Diagram
![SalesDataPipeline Architecture](https://github.com/yash872/Event-Driven-Sales-DataPipeline/blob/main/Images/SalesDataPipeline.jpg)

***

## Key Steps
### 1. Create a State Machine
- We will create a State Machine "Sales-Data-StateMachine" using the Step Function service, 
which will start a workflow for the complete data process.
![stateMachine](https://github.com/yash872/Event-Driven-Sales-DataPipeline/blob/main/Images/stateMachine.JPG)

### 2. Create a S3 Bucket
- we will create a S3 bucket "orders-data-input-yb" for orders data json files upload
![S3](https://github.com/yash872/Event-Driven-Sales-DataPipeline/blob/main/Images/S3.JPG)


### 3. Create a Event Bridge Rule
- we will create a Event Bridge rule "s3-orders-json-data" to trigger the step function when we got a new file upload in S3 bucket.
![EventRule](https://github.com/yash872/Event-Driven-Sales-DataPipeline/blob/main/Images/EventRule.JPG)

- Note: We have to Enable from the S3 bucket to send Notification to Event Bridge.
![S3EventNoti](https://github.com/yash872/Event-Driven-Sales-DataPipeline/blob/main/Images/S3EventNoti.JPG)


### 4. Create a Lambda Funaction
- we will create a simple Lambda function "order-json-data-process" to validate the intrim data in state machine.
it will return the successfull output if the input data is having 'contaxt-info' else it will raise an exception

![validationLambda](https://github.com/yash872/Event-Driven-Sales-DataPipeline/blob/main/Images/validationLambda.JPG)

- Current State Machine workflow
![stateMachine2](https://github.com/yash872/Event-Driven-Sales-DataPipeline/blob/main/Images/stateMachine2.JPG)


### 5. Create a DynamoDB Table
- we will create a DynamoDB Table "valid-orders-data" to store the valid orders process by the lambda function.
![DynamoTable](https://github.com/yash872/Event-Driven-Sales-DataPipeline/blob/main/Images/DynamoTable.JPG)


### 6. Create a SQS
- we will create a SQS "order-data-dlq" to store the failed data messages for reprocessing purpose if required.
![dlq](https://github.com/yash872/Event-Driven-Sales-DataPipeline/blob/main/Images/dlq.JPG)

### 7. Complete the State Machine Flow
- we will complete the State Machine workflow by adding the DynamoDB and SQS.
![stateMachine_final](https://github.com/yash872/Event-Driven-Sales-DataPipeline/blob/main/Images/stateMachine_final.JPG)

### 8. Final Execution 
- we will upload the 'order-data.json' file in the S3 bucket, which will trigger the State Machine workflow.
- we are having 2 records in the current data to test both the case of having contact info in the data.
- Success
    - the record in which 'contact-info' is available will ingested as valid record in DynamoDB table.
        ![dynamoTrigger](https://github.com/yash872/Event-Driven-Sales-DataPipeline/blob/main/Images/dynamoTrigger.JPG)

    - we can see the record is inserted into the DynamoDB table.
        ![dynamoData](https://github.com/yash872/Event-Driven-Sales-DataPipeline/blob/main/Images/dynamoData.JPG)

- Failure
    - the record in which 'contact-info' is not available will be caught as exception an the output will be sent to SQS for review.
        ![sqsTrigger](https://github.com/yash872/Event-Driven-Sales-DataPipeline/blob/main/Images/sqsTrigger.JPG)
        
    - we can see the failed record is in the SQS.
        ![sqsData](https://github.com/yash872/Event-Driven-Sales-DataPipeline/blob/main/Images/sqsData.JPG)




