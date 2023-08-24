# Setup


### Amazon Web Service AWS
<br><br>
Open an account with AWS - you can do this under the following [link](https://aws.amazon.com/). Enter your details - the login process should be self-explanatory. Since AWS is chargeable, you will also be asked for your payment method. The easiest way is to store your credit card.
<br><br>
![Alt Image Text](./Images/RP_Setup1.png "Setup1")

<br><br><br><br>

After you have registered, you will be taken to the AWS Home console. This gives you an overview of the most important points. For us, the following points are particularly relevant:
 * **Recently visited**: All tools we require for this project are marked.
 * **Cost and usage**: Gives you an overview of the running costs of your applications.
<br>
We now start with setting up the database. Please enter **S3** in the search field above. 
<br><br>
![Alt Image Text](./Images/RP_Setup2.png "Setup2")

<br><br><br><br>

We now open an S3 bucket to store the data loaded by Alpha Vantage. **Create bucket**.
<br><br>
![Alt Image Text](./Images/RP_Setup3.png "Setup3")

<br><br><br><br>

Give your bucket a name - in this example I call the bucket ```api.alphavantage.data```. Please consider the rules für bucket naming (min. 3 characters, only lowercase letters, must begin or end with letter or number). 
<br>
Select the AWS Region. This allows you to select the location where your data is to be stored. Select your nearest location - the nearest location from Switzerland is Frankfurt.
<br><br>
![Alt Image Text](./Images/RP_Setup4.png "Setup4")

<br><br><br><br>

Leave the default selections:
 * **ACLs disabled**
 * **Block all public access**
<br><br>
![Alt Image Text](./Images/RP_Setup5.png "Setup5")

<br><br><br><br>

For the further selection, leave the standard selection as well. **Create bucket**
<br><br>
![Alt Image Text](./Images/RP_Setup6.png "Setup6")

<br><br><br><br>

After the bucket has been successfully created, we now create a second one. The second bucket is needed so that Athena (database) can write down the data on a folder. 
<br>
Open another bucket - **Create bucket**. This time give the name ```api.athena.data```. Otherwise, make the same selections as for the previous bucket.
<br><br>
![Alt Image Text](./Images/RP_Setup7.png "Setup7")

<br><br><br><br>

If you have also opened the second bucket, your Amazon S3 menu should look like the image below.
<br>
We now start with setting up the script for loading the data form Alpha Vantage. Please enter **Lambda** in the search field. 
<br><br>
![Alt Image Text](./Images/RP_Setup8.png "Setup8")

<br><br><br><br>

Create now a new function. **Create function**.
<br><br>
![Alt Image Text](./Images/RP_Setup9.png "Setup9")

<br><br><br><br>

To create a function do the following:
1. Select *Author from scratch*
2. Give your function a name - in this example ```api_alphavantage```
3. Select the most current Python version - in this case it is *Python3.11*
4. Select the architecture *x86_64*
5. **Create function**
<br><br>
![Alt Image Text](./Images/RP_Setup10.png "Setup10")

<br><br><br><br>

Before we enter our script we test the application. Click on **Test**.
<br><br>
![Alt Image Text](./Images/RP_Setup11.png "Setup11")

<br><br><br><br>

Configure the test event. Name the event. Beside that, nothing has to be adjusted. **Save**.
<br><br>
![Alt Image Text](./Images/RP_Setup12.png "Setup12")

<br><br><br><br>

Now click on **Test** again. A new tab opens in the *Code source* menu. If you get the the same message as in the image below, everything is set up correctly so far. 
<br><br>
![Alt Image Text](./Images/RP_Setup13.png "Setup13")

<br><br><br><br>

Before we continue with setting up our Lambda function, we need to make sure that Lambda has access to our S3 bucket. The easiest way to do this is to duplicate the existing web window tab so that you can directly edit your Lambda function afterwards.
<br>
1. Dublicate window tab
2. Enter **IAM** in the search field
3. Go now to **Policies**
4. Create a new policy. **Create policy**
<br><br>
![Alt Image Text](./Images/RP_Setup14.png "Setup14")

<br><br><br><br>

Now proceed as follows to set up the permission.
1. Click on **JSON**
2. Enter the following code:
   ```
   {
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "s3:*",
            "Resource": "*"
        }
    ]
   }
   ```
   This gives your Lambda function full permission for all S3 related actions in all buckets of your account, which is not optimal for security reasons. It is generally a best practice to follow the principle of least privilege. However, for this test case we use this code as it is simpler. 
3. **Next** 
<br><br>
![Alt Image Text](./Images/RP_Setup15.png "Setup15")

<br><br><br><br>

The next code is for loading the data from the database into a csv file. This can then simply be downloaded. Copy the following code.
<br><br>
```
# Export database to csv

import csv
from replit import db

# Filename for the CSV file
filename = "exported_data.csv"

# Open CSV file for writing
with open(filename, 'w', newline='') as csvfile:
    # Create CSV writer object
    csvwriter = csv.writer(csvfile)
    
    # Write CSV header
    headers = ["Symbol", "Open", "High", "Low", "Price", "Volume", "Latest Trading Day", "Previous Close", "Change", "Change Percent", "Timestamp"]
    csvwriter.writerow(headers)
    
    # Iterate through all keys in the database
    for key in db.keys():
        quote = db[key]
        row = [
            quote.get("01. symbol", ""),
            quote.get("02. open", ""),
            quote.get("03. high", ""),
            quote.get("04. low", ""),
            quote.get("05. price", ""),
            quote.get("06. volume", ""),
            quote.get("07. latest trading day", ""),
            quote.get("08. previous close", ""),
            quote.get("09. change", ""),
            quote.get("10. change percent", ""),
            quote.get("timestamp", "")  # Adding the timestamp
        ]
        
        # Write row to CSV
        csvwriter.writerow(row)
```
<br><br><br><br>

Now add this code under the existing code in the script. **Run** the script.
<br><br>
![Alt Image Text](./Images/RP_Setup16.png "Setup16")

<br><br><br><br>

On the left side under **Files** the file **exported_data.csv** has now been created. If you click on it, you will see the data contained in this csv. You can now download this csv for your further use.
<br><br>
![Alt Image Text](./Images/RP_Setup17.png "Setup17")

<br><br><br><br>

Now set this code to inactive as well by using the ```'''``` characters.
<br><br>
![Alt Image Text](./Images/RP_Setup171.png "Setup171")

<br><br><br><br>

The next code we need is if we want to delete the contents of the database. Again, copy the code below.
<br><br>
```
# Delete data in database

from replit import db

# Iterate through all keys in the database and delete each key
for key in db.keys():
    del db[key]

# Delete csv file manually
```

<br><br><br><br>

1. Click on **Database** under **Tools** at the bottom left.
2. A tab **Database** opens on the right-hand side next to Console - there you will see that there are **keys** in the database. This is the number of data records contained in the database.
3. Now copy the code into the script and execute it. **Run**.
<br><br>
![Alt Image Text](./Images/RP_Setup18.png "Setup18")

<br><br><br><br>

When you have executed the script, the data in the database should be deleted after a few seconds. Check this under **Database**.
<br><br>
![Alt Image Text](./Images/RP_Setup19.png "Setup19")
<br><br>
This way you can delete the contents of the database. You have to delete the csv file manually. Left-click on the file and delete.

<br><br><br><br>

Again, set this code to inactive by using the ```'''``` characters.
<br><br>
![Alt Image Text](./Images/RP_Setup20.png "Setup20")

<br><br><br><br><br>

We now have the codes for the following tasks in our script:
<br>
 * Manual download of the data
 * Viewing the database
 * Creation of a csv file
 * Deleting the database
<br>
What is missing now is the code to run the script automatically on a daily basis. Once again, make sure that all codes are set to inactive and then we'll get started. 

<br><br><br><br><br>

The following code now forms the core of our script. This code can no longer be executed manually, but is triggered via the web. More about this later. Copy the code.
<br><br>
```
# Start via CRONJOB.DE (automatic execution)
# manual start via Web; https://apistockmarketdata.samuelhaller.repl.co/fetchdata

import requests
import time
from replit import db
from flask import Flask
from datetime import datetime  # Importing the datetime module

app = Flask(__name__)

def load_symbols_from_file(filename):
    with open(filename, 'r') as file:
        return [line.strip() for line in file]

def load_api_key_from_file(filename):
    with open(filename, 'r') as file:
        for line in file:
            key, value = line.strip().split('=')
            if key == "API_KEY":
                return value
    return None

symbols = load_symbols_from_file('symbols.txt')
apikey = load_api_key_from_file('config.txt')

def fetch_data():
    for symbol in symbols:
        url = f'https://www.alphavantage.co/query?function=GLOBAL_QUOTE&symbol={symbol}&apikey={apikey}'
        r = requests.get(url)
        data = r.json()
        
        quote = data.get("Global Quote", {})
        trading_day = quote.get("07. latest trading day", "")
        
        # Combine symbol and trading day into a unique key
        unique_key = f"{symbol}_{trading_day}"
        
        # Add the current timestamp
        quote['timestamp'] = datetime.now().strftime('%Y-%m-%d %H:%M:%S')
        
        # Check if the key is already in the database
        if unique_key not in db:
            db[unique_key] = quote

        print(data)
        time.sleep(15)

@app.route('/')
def home():
    return "Welcome to my Flask app!"

@app.route('/fetchdata')
def fetch_data_route():
    fetch_data()
    return "Data fetching completed!"

@app.route('/cronjob_78641.html')
def cron_verification():
    return "cronjob.de"

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=8080)
```
<br><br>
This script includes the following items:
1. Import all functions which are required - including Flask. Due to this package the code is executed via web.
2. Loads the created supporting files.
3. Downloads the data from all the symbols stored in symbols.txt.
4. Safes the data in the database.
5. Only data is saved which is not in the database yet.
6. Depending on the web entries, different returns are triggered - more about that later.
7. A timestamp is added with the time when the data was added to the database.
8. Since Alpha Vantage has a restriction (only 5 requests per minute) -> there is a time.sleep function which ensures that data is only loaded from one share every 15 seconds. Thus, no more than 4 requests are made per minute. 

<br><br><br><br>

Now copy the code into the script. As this code is the most important, copy the code at the beginning - 1st line.
<br><br>
![Alt Image Text](./Images/RP_Setup21.png "Setup21")

<br><br><br><br>

In order for this code to be called via external web, the script must always be running. In other words, the cloud server must be online so that the script can download the data from Alpha Vantage at all time. To ensure that the script is always running and does not automatically go offline after some time, we must activate **Always On**. To do this, click on the blue Python symbol at the top right. In order to activate this, you must either buy "My Cicles" or make a paid subscription. It is recommended to make a **Hacker** subscription. With this subscription you also get more storage space in your database. You can find the prices [here](https://replit.com/pricing).
<br><br>
If you have bought the "My Cicles" or made the subscription, activate the **Always On** function.
<br><br>
![Alt Image Text](./Images/RP_Setup22.png "Setup22")

<br><br><br><br>

Now click on **Run**. A webview will now open on the right-hand side. You will now see a link with your project und user name. Copy this link and write it into your code in the script (line 2 in the image below).  Add ```Https://``` to the front of the code and add ```/fetchdata``` to the back. <br> **This is the code with which you execute the script.**  
<br><br>
![Alt Image Text](./Images/RP_Setup24.png "Setup24")
<br><br><br><br>

In summary, the following can be said:
<br><br>
Code: **Https://apistockmarketdata.samuelhaller.repl.co**    (acc. Replit webview)
  * Checks if the script runs.
  * *Can be triggered when script is executed in Replit (**Run**) or the link is entered on the web.*
  * Gives as response "Welcome to my Flask app!"
<br><br>  
Code: **Https://apistockmarketdata.samuelhaller.repl.co/fetchdata**
  * Is executing the script and downloads the stock data.
  * *Can only be executed via web.*
  * Gives as response "Data fetching completed!"

<br><br>
Example of what it looks like when triggered via the web.
<br><br>
![Alt Image Text](./Images/RP_Setup241.png "Setup241")
<br>
Please note, this is only working when the script in Replit is **running**.

<br><br><br><br>

### CRONJOB.DE
<br><br>
To ensure that the script now runs daily, we now call up the URL-link (Https://apistockmarketdata.samuelhaller.repl.co/fetchdata - *your URL is different*) daily with the help of CRONJOB.DE. To do this, we have to register on CRONJOB.DE. The page is in German. You can also use another cronjob provider if you want to have an English page. There are many providers in this area. 
<br>
Register yourself on [CRONJOB.DE](https://www.cronjob.de/anmeldung).
<br><br>
![Alt Image Text](./Images/RP_Setup26.png "Setup26")

<br><br><br><br>

In the **Home** menu, click on **Cronjobs**.
<br><br>
![Alt Image Text](./Images/RP_Setup261.png "Setup261")

<br><br><br><br>

Click on the button **Neuen CRONJOB anlegen**.
<br><br>
![Alt Image Text](./Images/RP_Setup27.png "Setup27")

<br><br><br><br>

1. Name your CRONJOB
2. Enter your URL address - make sure that the URL starts with **https://** and ends with **/fetchdata**. See example in the figure below.
3. Define the scheudle
4. Safe CRONJOB. **CRONJOB speichern**
<br><br>
![Alt Image Text](./Images/RP_Setup28.png "Setup28")

<br><br><br><br>

After setting up the cronjob, you will be prompted to run a verification. This is asked to ensure that you are authorised to create this cronjob for the relevant server (Replit). 
<br>
In this example, we must now create an html file in Replit with the name **cronjob_788641.html**. This file should contain the content **cronjob.de**. Before you click on the **Prüfung jetzt durchführen** button, you can check manually with the shown link (**Https://apistockmarketdata.samuelhaller.repl.co/cronjob_78641.html**) whether the verification works. We do this at a later step when the Html file is created.  
<br><br>
![Alt Image Text](./Images/RP_Setup281.png "Setup281")

<br><br><br><br>

Go back to your Repl and open a html file. In my case it is called **cronjob_78641.html** - you will have a different number. Then open the file and put ```cronjob.de``` in it. 
<br><br>
![Alt Image Text](./Images/RP_Setup29.png "Setup29")

<br><br><br><br>

Now go back to the script (**main.py**) and scroll down in the code to **@app.route('/cronjob_78641.html')**. Now change the number to match the title of your html file. Then click on **Run**.
<br><br>
![Alt Image Text](./Images/RP_Setup30.png "Setup30")

<br><br><br><br>

Now enter your link (in my case:**Https://apistockmarketdata.samuelhaller.repl.co/cronjob_78641.html**) which was shown in CRONJOB.DE in the web browser. If the result is **cronjob.de**, everything works. Otherwise, check the previous steps again.
<br><br>
![Alt Image Text](./Images/RP_Setup31.png "Setup31")

<br><br><br><br>

If it worked, you can now run the check in CRONJOB.DE. **Prüfung jetzt durchführen**
<br><br>
![Alt Image Text](./Images/RP_Setup32.png "Setup32")
<br>
Afterwards, a message appears if it has worked.

<br><br><br><br>

The setup is now complete and everything should work. It is now advisable to delete the previous data so that the download is then only carried out automatically via the URL. However, this step is optional and does not necessarily have to be carried out. Please make sure that only the code related to deleting the database is active in the script. All other code must be set to inactive. 
<br><br>
![Alt Image Text](./Images/RP_Setup33.png "Setup33")
<br>
Afterwards, check whether all keys have been deleted from your database.

<br><br><br><br>

**As a last step**: Make sure that only the code needed for the automatic download of the data via cronjob is activated. Also make sure that the **script is running**. If the script is not running, nothing can be triggered via the web. 
<br><br>
![Alt Image Text](./Images/RP_Setup34.png "Setup34")

<br><br><br><br><br><br>

## Here a quick summary about the setup

<br><br><br>
### Web links

<br>

Code: **Https://apistockmarketdata.samuelhaller.repl.co**    (acc. Replit webview)
  * Checks if the script runs.
  * *Can be triggered when script is executed in Replit (**Run**) or the link is entered on the web.*
  * Gives as response "Welcome to my Flask app!"

<br>

Code: **Https://apistockmarketdata.samuelhaller.repl.co/fetchdata**
  * Is executing script and downloads data.
  * *Can only be executed via the web.*
  * Gives as response "Data fetching completed!"

<br>

Code: **Https://apistockmarketdata.samuelhaller.repl.co/cronjob_78641.html**
  * Is required for the verification in CRONJOB.DE.
  * Must be performed once.
  * *Can only be executed via the web.*
  * Gives as response "Cronjob.de"
<br><br>
*Please have in mind, that your URL link looks different since the code is depending on your project and user name (and verification number from CRONJOB.DE).*

<br><br><br>
### Python script - different codes

<br>

In our script we have the following codes:
<br>
1. Automatic download of data via cronjob
2. Manual download of the data
3. Viewing the database
4. Creation of a csv file
5. Deleting the database

<br><br>

Make sure of the following:
 * That the script is **always** running. If the script is not running, nothing can be triggered via the web.
 * That only the first code (**Automatic download of data via cronjob**) is set to active.






