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
