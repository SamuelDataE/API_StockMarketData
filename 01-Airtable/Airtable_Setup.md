  ## General information about Airtable
Airtable is a cloud-based collaboration platform that combines the functionality of a spreadsheet with a database. It allows users to create and organize information in customizable tables, making it easy to track, manage, and collaborate on various types of data and projects. In our particular case, it offers the advantage that you don't have to programme anything yourself.

The following figure shows how the process looks for the daily download of the data.

1. The data is provided by [Alpha Vantage](./00.%20Alpha_Vantage). They are stored in the Airtable database using Alpha Vantage's API. 
2. Airtable add-on application Data Fetcher is needed to run this job automatically on a daily basis. 
3. The daily stock market data of all the stocks we are searching for are now always loaded into an Airtable table. This data is then loaded daily into a second Airtable table using the second add-on tool Zapier. In this way, we collect the data there in order to be able to accumulate a history of the respective shares. 

![Alt Image Text](./Images/Airtable_dataflow1.png "Dataflow")
  

# Airtable setup
