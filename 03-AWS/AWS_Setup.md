# Setup


### Airtable
<br><br>
Open an account with Airtable - you can do this under the following [link](https://airtable.com/signup). Enter your details - the login process should be self-explanatory. 
<br><br>
![Alt Image Text](./Images/Airtable_Login1.png "Login")

<br><br><br><br>

When you are logged in, the app looks like the image below. Now open a new **base**.
<br><br>
![Alt Image Text](./Images/Airtable_Setup.png "Setup")

<br><br><br><br>

When a new base is opened, Airtable creates an Excel-like interface with a table. In the first column of the table, the symbols of the respective shares of which you want to build a history can be entered. How to determine the symbols of the respective shares is described under [Alpha-Vantage_Stock Selection](../00-Alpha_Vantage/Alpha-Vantage_Stock-Selection.md).
<br><br>
![Alt Image Text](./Images/Airtable_Setup1.png "Setup1")

<br><br><br><br>

In order to download the data from Alpha Vantage, the add-on application Data Fetcher is required in Airtable. This tool can be added via **extensions**.
<br><br>
![Alt Image Text](./Images/Airtable_Setup2.png "Setup2")

<br><br><br><br>

Search for the tool **Data Fetcher** and add the application.
<br><br>
![Alt Image Text](./Images/Airtable_Setup3.png "Setup3")

<br><br><br><br>

Register with the Data Fetcher application. There is a basic version which is free of charge. 
<br><br>
![Alt Image Text](./Images/Airtable_Setup4.png "Setup4")

<br><br><br><br>

Start your first request with Data Fetcher.
<br><br>
![Alt Image Text](./Images/Airtable_Setup5.png "Setup5")

<br><br><br><br>

1. Name the process
2. Select **Custom**
3. Enter the following Link
   ```
   https://www.alphavantage.co/query?function=SYMBOL_SEARCH&keywords=EMMI&apikey=demo
   ```
4. Replace **demo** with your personal API key. How to get an API Key is explained [here](../00-Alpha_Vantage/Alpha-Vantage_General-Information.md).
5. Click on the **+** sign next to IBM. 
<br><br>
![Alt Image Text](./Images/Airtable_Setup6.png "Setup6")

<br><br><br><br>

1. Select the respecitve table in the drop down selection. If you didn't change the name manuelly its called **Table 1**.
2. Select the column/field name where you have entered the symbols of your shares. If you didn't change the field manually its called **Name**.
3. Confirm selections.
<br><br>
![Alt Image Text](./Images/Airtable_Setup7.png "Setup7")

<br><br><br><br>

Set up the scheduled request. The data should be requested every day in the morning at 0900am. In other words, the table with the data is updated each morning.    
<br><br>
![Alt Image Text](./Images/Airtable_Setup8.png "Setup8")

<br><br><br><br>

Each listed stock symbol generates one **run** daily. However, only 100 *runs** are included in the free basic version. It is therefore very quickly necessary to upgrade. This can be done directly at Data Fetcher under [Pricing](https://airtable.com/signup](https://datafetcher.com/). The costs for the respective versions are as shown in the figure below. 
<br><br>
![Alt Image Text](./Images/Airtable_Setup9.png "Setup9")

<br><br><br><br>

1. After setting up the schedule request - click on **run**
2. New Windows opens - click on **continue**
<br><br>
![Alt Image Text](./Images/Airtable_Setup10.png "Setup10")

<br><br><br><br>

1. New Windows opens - click on **Filter all**.
2. Select all fields/columns with the information you want to have about the shares.
   In this example the following fields have been selected:
   > Global Quote symbol
   > Global Quote price
   > Global Quote volume
   > Global Quote latest trading day 
3. Click on **Save & run**
<br><br>
![Alt Image Text](./Images/Airtable_Setup11.png "Setup11")

<br><br><br><br>

New window opens - click on **Show output table**   
<br><br>
![Alt Image Text](./Images/Airtable_Setup12.png "Setup12")

<br><br><br><br>

We are now back in the original mask of our **base**. The table has now been completed with the new columns accoring to our request. 
1. Delete the columns that we no longer need.
2. Add a new column **Last modified time** 
<br><br>
![Alt Image Text](./Images/Airtable_Setup13.png "Setup13")

<br><br><br><br>

Choose an name for the new column and select **All editable fields**. Then click on **Create filed**.
<br><br>
![Alt Image Text](./Images/Airtable_Setup14.png "Setup14")

<br><br><br><br>

The column should now look like the illustration below.
<br><br>
![Alt Image Text](./Images/Airtable_Setup15.png "Setup15")

<br><br><br><br>

Now add a new table and give it a name. Save it.
<br><br>
![Alt Image Text](./Images/Airtable_Setup16.png "Setup16")

<br><br><br><br>

Adjust the column names as they appear in Table 1. Also adjust the format in the respective columns (e.g. word or number).
<br><br>
![Alt Image Text](./Images/Airtable_Setup161.png "Setup161")

<br><br><br><br>

### Zapier
<br><br>
In order to be able to add the data from table 1 to our new table, we need the application Zapier. As soon as the data in the first table is updated, this data is added to the second table. The existing data is not overwritten but added. In this way, we build up a database of the respective shares.

Register yourself at [Zapier](https://zapier.com/app/login). Enter your personal details.
![Alt Image Text](./Images/Airtable_Setup17.png "Setup17")

<br><br><br><br>

In the registration process you will be asked for which applications you use Zapier. Select **Airtable**.
<br><br>
![Alt Image Text](./Images/Airtable_Setup18.png "Setup18")

<br><br><br><br>

After the registration is completed you will be able to **create Zap**. Click on the button.
<br><br>
![Alt Image Text](./Images/Airtable_Setup19.png "Setup19")

<br><br><br><br>

A new page opens with an illustration of the process flow. Click on **1. Untitled Step**.
<br><br>
![Alt Image Text](./Images/Airtable_Setup20.png "Setup20")

<br><br><br><br>

In the new window that opens, select the **Airtable** option.
<br><br>
![Alt Image Text](./Images/Airtable_Setup21.png "Setup21")

<br><br><br><br>

A new selection bar opens on the right-hand side. Select **New or updated record** as the event. Continue.
<br><br>
![Alt Image Text](./Images/Airtable_Setup22.png "Setup22")

<br><br><br><br>

You will now be asked to link your Airtable profile to Zapier. Link the accounts and **Grant access**. 
<br><br>
![Alt Image Text](./Images/Airtable_Setup23.png "Setup23")

<br><br><br><br>

After connection your accounts click on **contine**. 
<br><br>
![Alt Image Text](./Images/Airtable_Setup24.png "Setup24")

<br><br><br><br>

After connection your accounts click on **contine**. 
<br><br>
![Alt Image Text](./Images/Airtable_Setup25.png "Setup25")







