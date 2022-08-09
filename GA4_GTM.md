# Get started with Google Analytics 4

1. Create Google Analytics Account

- go to https://analytics.google.com/ and click on **Get Started**

  - If you already have an account  
    go to https://analytics.google.com/ and click on **Admin > Create Account**.

- Account setup
  - Account Name: It can be anything e.g. your organisation name
  - Select **Benchmark, Technical support, Account Specialists** and click **Next**.
- Property setup
  - Enter **Property name**.
  - Select **Reporting time zone** and **Currency** and click **Next**.
  - And click **Create**.

Once you are done with this, you will get 3 options Web, iOS app, Android app. select **Web**.

- In the field Website URL add the URL of your website, and **if you are running it on localhost just enter the any website URL** (It has nothing to do with anything if you are using localhost), everything will be working on the basis of where you have added the tracking code. Similary add **Stream name**, and click **Create Stream**.

- Now go to **Admin** from left navigation bar and click on **Data Stream**,Select the stream you have created, copy **MEASUREMENT ID** (we will be needing it in Google Tag Manager)

- From the same page, in **Tagging Instructions** click on **Global site tag (gtag.js)** and copy the code and paste it in global index page of your website's frontend code.

Now your basic settings for Google analytics 4 is done.

2. Create Google Tag

- for detailed Google Tag Manager introduction

  - https://www.youtube.com/watch?v=1dwk_erXAko&t=0s

- ## Steps:

  > - Go to https://tagmanager.google.com/
  > - Click **Start for free** and create your account & if you already have Google Tag Manager account then just click on **Add Account**.
  > - In case of **Container Setup > Container Name** it doesn't have to be your website URL, it can be name as well.
  > - Choose **Target Platform** as **Web** & click **Create**.
  > - Now once you are on **Home page** of **Google Taag Manager** click on the **Containet ID**. You get the code to install GTM on your website.
  > - Copy that code snipet and paste it into your code source (in head and body section).
  > - To check if you have successfully installed GTM on your project just click **Preview** on your Tag Manager homepage.
  > - Enter your **URL** and click **Start**.
  > - After that if you see **Debugger Connected Widget** which says **Connected**, you have successfully installed the GTM on your project.
  > - Now go to your Google Analytics, and click on **Gear icon**, Select **Data Stream**, select your **Project** and copy the **MEASUREMENT ID**.
  > - Now go back to **GTM**, Click on **Tags** in navigation bar & click **New**.
  > - Click on **Tag Configuration** and select **Google Analytics: GA4 Configuration** and paste your **MEASURMENT ID**.
  > - Since we want our tag to get fired on every page we have, **Add Trigger**, for that click on **Triggering** adn select **All Pages**, Enter Tag name, add click **Save**.

- But our website is **Single Page Application** and we want to track each individual user we have to add one more trigger i.e. **History Change**.
  - **History Change** is the event which get triggered whenever the URL of your page change.

### Create History Change Trigger

- Go to **Triggers**, click **New > Trigger Configuration**, Search for **History Change**, enter the desired name for the Trigger and click **Save**.

  > - Now go to **Tags**, Select the tag you have created earlier, click on **Triggerings** and add **History Change** trigger you just created.
  > - To check whether your tag and triggers are working go to **Google analytics > Configure > DebugView**, And check whether you are getting fields like **Page views, scrolls** and etc.

For tracking Individual User, we need to have **Login Functionality** in our website, this is needed because with the help of login we can identify the user since every user have **unique identification** in our database.  
And our main aim is to **pass this ID** to **Google Analytics** so it can track user data and bind it to that particular userId (Even if the user uses different devices to login).

## Adding USERID in DataLayer

**_Code to push userID to DataLaayer_**

```javascript
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  event: "login",
  userId: customUserId, // It is aprimary key from your database.
});
```

**Add the above code logic in your source code in login functionality**

## Create a DataLayer Variable (i.e. userId)

> - Go to **Google Tag Manager > Variables > User-defined Variables** and click **New**.
> - Click **Variable Configuration > DataLayer Variable**.
> - In the field Data Layer Variable name **Use the exact same variable name as you have passed from your source code** (In our case its userId) & click **Save**.

> - Now go back to **Tags Section**, Select the tag, click **Tag Configuration**, click **Fields to set > Add Row**.
> - Enter the **Field Name** as **user_id** ( This the parameter Google Analytics expect for **User Tacking**) & in the **Value Section** select the Data Layer Variable you have created. Click **Save**.
> - And after this just **Submit** the tag, by clicking **Submit** and Entering **Version Name** and click **Publish**.

If we want to use **User ID** you have passed as a dimension in your **Reports**, we need create custom dimension in Google Analytics.

## Create User Scope Custom Dimension

> - In **Google tag Manager**, go to **Tags**, select your **GA4 Configuration tag > User Properties > Add Row** & enter the Parameter name other than name you have used for User ID, (Use something like **user_id_dim**) & in value section **use the variable you have created earlier** & click **Save**.
> - Go to **Google Anlytics**, choose **Custom definations > Craete custom dimensions**.
> - Enter dimension name (It can be anything).
> - Select **Scope** as **User**.
> - Enter the description.
> - In **User Property** enter the exact same name a syou have used in **User properties** (in our case its **user_id_dim**) & Click **Save**.

And within next 24 hours you will be able to use the User ID in your Reports.

**To use user defined variables you have to add those in _fields to set_ and _user properties_ in your GA4 Configuration TAG**
