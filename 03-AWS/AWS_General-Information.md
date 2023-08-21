# General information about Amazon Web Service
<br><br>
Replit.io, commonly referred to as Replit, is an online platform that offers an integrated development environment (IDE) where users can write, run, and share code in various programming languages directly in the browser. It facilitates collaborative coding, hosting web apps, and provides a range of educational resources for learners. Replit's mission is to make programming more accessible to a wider audience. Since it is an integrated development enivronment, users can access various services via one platform and do not have to worry about hosting or maintaining the different services. 

The following figure shows how the process looks for the daily download of data via API.

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
