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
   This gives your Lambda function full permission for all S3 related actions in all buckets of your account, which is not optimal for security reasons. It is generally a best practice to follow the principle of least privilege (only as much as necessary). However, for this test case we use this code as it is simpler. 
3. **Next** 
<br><br>
![Alt Image Text](./Images/RP_Setup15.png "Setup15")

<br><br><br><br>

Now give the policy a name (e.g. ```Full_Access_S3_Lambda```) and write down a description (e.g. ```Full access for Lamdba in S3```). After that **Create policy**.
<br><br>
![Alt Image Text](./Images/RP_Setup16.png "Setup16")

<br><br><br><br>

Go back to the IAM menu and click on the left side on **Roles**. Select your Lambda Function - the role starts with the name of your function. In this case its is *api_alphavantage-role-aju6yhkx*.
<br><br>
![Alt Image Text](./Images/RP_Setup17.png "Setup17")

<br><br><br><br>

Under *Permissions* you can now click on **Add permissions**. 
1. Choose **Attach policies**
2. Select your created policy (e.g. *Full_Access_S3_Lambda*)
3. **Add permissions**
<br>
Once you have added the respective policy, it should appear in your list. 
<br><br>
![Alt Image Text](./Images/RP_Setup18.png "Setup18")

<br><br><br><br>

We use Python to download the data. In Python we use the *requests* library (an already prefabricated Python package). In AWS Lambda, not all Python libraries are available by default. Therefore, you need to provide them together with your Lambda code. For this we need to do the following steps:
1. Open your local *Command Prompt*. To do this, click **Win + R** on your keyboard. Windows open - enter **cmd**. **OK**.
2. Use the command **cd** followed by the path to navigate to the directory where you want to create a new directory. For example: **cd C:\Users\YourName\Documents**. In the image below, I have entered the path as it would look on my PC. When you have entered this. Press **Enter**.
3. A new line now appears and you can see that the working directory has now been changed according to my entry under step #2. 
<br><br>
![Alt Image Text](./Images/RP_Setup19.png "Setup19")


<br><br><br><br>

Now copy the following code into your *cmd*: ```mkdir my_lambda_package```. Press **Enter**.
<br><br>
![Alt Image Text](./Images/RP_Setup20.png "Setup20")

<br><br><br><br><br>

We have now opened the folder. Now we will download the *requests* library.
1. Change the working directory. You can do this by entering the following code into the console ```cd my_lambda_package```. Press enter.
2. Install the request library. Type ```pip install requests -t .``` and execute it by pressing enter.
<br><br>
![Alt Image Text](./Images/RP_Setup22.png "Setup22")

<br><br><br><br>

Now check in your folder **my_lambda_package** whether the download worked.
<br>
If you see these folders, it worked.
<br><br>
![Alt Image Text](./Images/RP_Setup23.png "Setup23")

<br><br><br><br>

Now open your tab with the AWS Lambda function.
1. Click on **Actions**
2. Select **Export function**
3. Choose **Download deployment package**  
<br><br>
![Alt Image Text](./Images/RP_Setup25.png "Setup25")
<br><br><br><br>

Now open the folder where the file was downloaded - probably in the *Downloads* folder - and copy the lambda_function.py file (*Ctrl + C*) and save it in your my_lambda_package folder (*Ctrl +V*).
<br><br>
![Alt Image Text](./Images/RP_Setup26.png "Setup26")

<br><br><br><br>

We now need to create a zip file.
1. Go to your *my_lambda_package* folder.
2. Mark all folders (*Ctrl + A*)
3. Left click and select **Compress to ZIP file**.
4. Give the zip file the name **my_Lambda_package.zip**.  
<br><br>
![Alt Image Text](./Images/RP_Setup24.png "Setup24")
<br>
The folder marked in green is the new zip file created.
<br><br><br><br>

Now go back to the Lambda function in AWS. 
1. Go to **Upload from**
2. Select **.zip file**
3. Click on **Upload**
4. Select your zip file (*my_lambda_package.zip)
5. **Save**
<br><br>
![Alt Image Text](./Images/RP_Setup27.png "Setup27")

<br><br><br><br>

Your *Code source* should now contain the same files as shown in the image below.
<br><br>
![Alt Image Text](./Images/RP_Setup28.png "Setup28")

<br><br><br><br>

Now we can place our Python code into the Lambda_function.py file. Copy the following code.
```
import requests
import time
import json
import boto3
import os
from datetime import datetime

# Initialisieren des S3-Clients
s3 = boto3.client('s3')
BUCKET_NAME = 'data.alphavantage'

# Lese Umgebungsvariablen
apikey = os.environ['API_KEY']
symbols = os.environ['SYMBOLS'].split(',')

def lambda_handler(event, context):
    for symbol in symbols:
        url = f'https://www.alphavantage.co/query?function=GLOBAL_QUOTE&symbol={symbol}&apikey={apikey}'
        r = requests.get(url)
        data = r.json()
        
        quote = data.get("Global Quote", {})
        trading_day = quote.get("07. latest trading day", "")
        
        # Kombinieren Sie Symbol und Handelstag zu einem eindeutigen Schlüssel
        unique_key = f"{symbol}_{trading_day}.json"
        
        # Überprüfen, ob der Schlüssel bereits in S3 ist
        try:
            s3.head_object(Bucket=BUCKET_NAME, Key=unique_key)
        except:
            # Fügen Sie den aktuellen Zeitstempel hinzu, wenn der Schlüssel nicht vorhanden ist
            quote['timestamp'] = datetime.now().strftime('%Y-%m-%d %H:%M:%S')
            s3.put_object(Bucket=BUCKET_NAME, Key=unique_key, Body=json.dumps(quote))
        
        print(data)
        time.sleep(12)
    return {
        'statusCode': 200,
        'body': json.dumps('Data processing completed!')
    }
```
<br>
Enter the code in AWS Lambda. You can overwrite / delete the previous code. 
<br><br>
![Alt Image Text](./Images/RP_Setup29.png "Setup29")

<br><br><br><br>

Enter the code in AWS Lambda. You can overwrite / delete the previous code. 
<br><br>
![Alt Image Text](./Images/RP_Setup29.png "Setup29")

<br><br><br><br>

Before we run the code, we need to do another configuration in AWS Lambda.
1. Go to **Configuration**
2. Click on **Environment variables**
3. Click on **Edit**
<br><br>
![Alt Image Text](./Images/RP_Setup30.png "Setup30")

<br><br><br><br>

In our Python code we refer to these environment variables. Therefore, we now need to define them.
1. The first variable is your API key. For this, enter ```API_Key``` in the key name. Now enter your API Key from Alpha Vantage in Value. How to request that Key is described [here](../00-Alpha_Vantage/Alpha-Vantage_General-Information.md).
2. The second variable are your symbols. Enter in 
<br><br>
![Alt Image Text](./Images/RP_Setup30.png "Setup30")

<br><br><br><br>









