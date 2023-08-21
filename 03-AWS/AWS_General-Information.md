# General information about Amazon Web Service
<br><br>
AWS, or [Amazon Web Services](https://aws.amazon.com/de/), is a subsidiary of Amazon providing on-demand cloud computing platforms and APIs to individuals, companies, and governments. It offers a broad set of tools and services including computing power, storage, and databases. AWS enables its users to run applications, host websites, and manage big data on a scalable and reliable infrastructure. For our project, we only need services from AWS in addition to the external data provider [Alpha Vantage](https://www.alphavantage.co/#page-top).

The following figure shows which applications are used for the daily download of data via API in AWS.

1. As with the other approches, the data is provided by the market data provider [Alpha Vantage](../00-Alpha_Vantage).
2. AWS is a common cloud-computing-platfomr which offers a whole range of different tools. For our aim we are using the following AWS applications:
    * **AWS Lambda**: Serverless computing service that automatically runs code in response to specific events without requiring the management of servers. Our python script will run with Lambda. 
    * **AWS IAM**: Service that allows users to control access to AWS resources in a secure manner.
    * **Amazon EventBridge**: Serverless event bus service that facilitates the connection of applications using data from various sources. Responsible in this project for running the code in Lambda on a daily basis. 
    * **AWS S3**: Scalable cloud storage service that allows users to store and retrieve data.
    * **AWS Athena**: Serverless interactive query service that enables analysis of data in Amazon S3 using SQL.
<br>

![Alt Image Text](./Images/AWS_Dataflow.png "Dataflow")
  
<br><br><br><br>

## Result
<br>
The end result is a database with daily records on the requested shares. 
<br><br>

![Alt Image Text](./Images/AWS_Result.png "Result")
<br><br>
This data can be downloaded in a csv file.

<br><br><br><br>

## Running costs
<br>
Pricing with AWS is more complex than with the other two methods. AWS pricing varies significantly across its vast range of services and is influenced by factors such as data volume, request frequency, and regional costs.
<br>

**Generally, users pay for what they use, with many services operating on a pay-as-you-go model.** 
<br><br><br>
Here is a list of the costs of each service:
<br><br>
### AWS Lambda
<br>![Alt Image Text](./Images/AWS_PricingLambda.png "PricingLambda")
<br>
Here the detailed costs of AWS Lambda: [Pricing](https://aws.amazon.com/lambda/pricing/)

<br><br>

### Amazon EventBridge
<br>![Alt Image Text](./Images/AWS_PricingAmazonEventBridge.png "PricingEventBridge")
<br>
Here the detailed costs of Amazon EventBridge: [Pricing](https://aws.amazon.com/eventbridge/pricing/)

<br><br>

### AWS S3
<br>![Alt Image Text](./Images/AWS_PricingS3.png "PricingS3")
<br>
Here the detailed costs of AWS S3: [Pricing](https://aws.amazon.com/s3/pricing/)

<br><br>

### AWS Athena
<br>![Alt Image Text](./Images/AWS_PricingAthena.png "PricingAthena")
<br>
Here the detailed costs of AWS Athena: [Pricing](https://aws.amazon.com/athena/pricing/)

<br><br>

### Personal cost overview
<br>
Even with this information above, it is very difficult to get an estimate of how the costs will behave for our project. Therefore, here is an overview of my costs:
<br><br>
Duration: 1 month
Number of shares: 10 pieces 
<br>

![Alt Image Text](./Images/AWS_Costs.png "PricingCosts")
