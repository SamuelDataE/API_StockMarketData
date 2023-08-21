# General information about Amazon Web Service
<br><br>
AWS, or [Amazon Web Services](https://aws.amazon.com/de/), is a subsidiary of Amazon providing on-demand cloud computing platforms and APIs to individuals, companies, and governments. It offers a broad set of tools and services including computing power, storage, and databases. AWS enables its users to run applications, host websites, and manage big data on a scalable and reliable infrastructure. For our project, we only need services from AWS in addition to the external data provider [Alpha Vantage](https://www.alphavantage.co/#page-top).

The following figure shows which applications are used for the daily download of data via API in AWS.

1. The data is provided by the market data provider [Alpha Vantage](../00-Alpha_Vantage).
2. Replit is the IDE which is hosting our python script. With this script the data is pulled out from Alpha Vantage and stored directly in the Replit database.  
3. With the external application [CRONJOB.DE](https://www.cronjob.de/) we are able to execute the python script daily.
4. In this way, the data is collected and cumulated to provide a history of the respective shares. 

![Alt Image Text](./Images/RP_Dataflow.png "Dataflow")
  
<br><br><br><br>

## Result
The end result is a database with daily records on the requested shares. 
<br><br>
![Alt Image Text](./Images/RP_Database.png "Result")
<br><br>
This data can be downloaded in a csv file.

<br><br><br><br>

## Running costs
<br>

| Application  | Free Version  | Note          |
|-----------   |---------------|---------------|
| REPLIT     | (yes)       | The "Hacker" subscription is required as the "Always On" functionality is required in the process. This subscription costs USD 7 per month. |
| CRONJOB.DE | yes         | |

<br><br>

Here the detailed costs of Replit: [Pricing](https://replit.com/pricing).  
<br>
![Alt Image Text](./Images/RP_Pricing.png "Pricing")
