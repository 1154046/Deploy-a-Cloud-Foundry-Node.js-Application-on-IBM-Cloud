# Deploy a Cloud Foundry Node.js Application on IBM-Cloud
By following this tutorial, you'll set up a development environment, deploy an app locally on IBM Cloud®, and integrate an IBM Cloud database service in your app.


## Before you begin
Note: This repo is part of the 2021 Call for Code events - https://www.crowdcast.io/e/cfc2021-ibmcloud


You'll need the following accounts and tools:

1. IBM Cloud account -  https://ibm.biz/BdfPVJ 
( Follow steps to get started with Call for Code )
3. IBM Cloud CLI - https://cloud.ibm.com/docs/cli?topic=cli-getting-started
4. Git - https://git-scm.com/downloads
5. Node - https://nodejs.org/en/


## Steps

### Step 1: Create a Cloud Foundry App Instance

1. Login to IBM Cloud or Create a Free Account: https://ibm.biz/BdfPVJ

2. Navigate to https://cloud.ibm.com/cloudfoundry/overview

Click on create to choose a runtime.
![Screenshot (632)](https://user-images.githubusercontent.com/20628307/118638951-ab04c400-b7d7-11eb-872b-8450e53282a6.png)

Choose Lite Plan Pricing Plan and navigate down.
![Screenshot (633)](https://user-images.githubusercontent.com/20628307/118638970-b0620e80-b7d7-11eb-985b-234ffb72d6b2.png)

Select the SDK for Node.js runtime and enter a unique name for your application.
![Screenshot (634)](https://user-images.githubusercontent.com/20628307/118638979-b3f59580-b7d7-11eb-8794-c4b603882c57.png)


### Step 2: Clone the sample app

First, clone the Node.js hello world sample app GitHub repo.

```
$ git clone https://github.com/IBM-Cloud/get-started-node
```


### Step 2: Run the app locally

Use the npm package manager to install dependencies and run your app.

1. On the command line, change the directory to where the sample app is located.
```
$ cd get-started-node
```

2. Install the dependencies listed in the package.json External link icon file to run the app locally. 
```
$ npm install
```

3. Run the app.
```
$ npm start
```


### Step 3: Prepare the app for deployment

To deploy to IBM Cloud, it can be helpful to set up a manifest.yml file. The manifest.yml includes basic information about your app, such as the name, how much memory to allocate for each instance and the route. We've provided a sample manifest.yml file in the get-started-node directory.

Open the manifest.yml file, and change the name from GetStartedNode to your app name, app_name.

```
applications:
- name: GetStartedNode
  random-route: true
  memory: 128M
```

### Step 4: Deploy the app

You can use the IBM Cloud CLI to deploy apps to IBM Cloud.

1. Log in to your IBM Cloud account, and select an API endpoint.
```
$ ibmcloud login 
```
If you have a federated user ID, instead use the following command to log in with your single sign-on ID. 
```
$ ibmcloud login --sso 
```

2. Target a Cloud Foundry org and space:
```
$ ibmcloud target --cf 
```
If you don't have an org or a space set up, see https://cloud.ibm.com/docs/account?topic=account-orgsspacesusers.

3. From within the get-started-node directory, push your app to IBM Cloud.
```
$ ibmcloud cf push 
```

Deploying your application can take a few minutes. When deployment completes, you'll see a message that your app is running. View your app at the URL listed in the output of the push command, or view both the app deployment status and the URL by running the following command:
```
$ ibmcloud cf push 
```

Step 5: Add a database

Next, we'll add an IBM Cloudant NoSQL database to this application and set up the application so that it can run locally and on IBM Cloud.

1. In your browser, log in to IBM Cloud and go to the Dashboard. Select Create resource.

2. Search for IBM Cloudant, and select the service.

3. For Available authentication methods, select Use both legacy credentials and IAM. You can leave the default settings for the other fields. Click Create to create the service.

4. In the navigation, go to Connections, then click Create connection. Select your application, and click Connect.

5. Using the default values, click Connect & restage app to connect the database to your application. Click Restage when prompted.

6. IBM Cloud will restart your application and provide the database credentials to your application using the VCAP_SERVICES environment variable. This environment variable is available to the application only when it is running on IBM Cloud.


```
TIP:
Environment variables enable you to separate deployment settings from your source code. For example, instead of hardcoding a database password, you can store it in an environment variable that you reference in your source code.
```

### Step 6: Use the database
We're now going to update your local code to point to this database. We'll create a JSON file that will store the credentials for the services the application will use. This file will get used ONLY when the application is running locally. When running in IBM Cloud, the credentials will be read from the VCAP_SERVICES environment variable.

1. In the get-started-node directory, create a file called vcap-local.json with the following content:

```
{
 "services": {
   "cloudantNoSQLDB": [
     {
       "credentials": {
         "url":"CLOUDANT_DATABASE_URL"
       },
       "label": "cloudantNoSQLDB"
     }
   ]
 }
}
```

2. Find your app in the IBM Cloud resource list External link icon. On the Service Details page for your app, click Connections in the sidebar. Click the IBM Cloudant menu icon (…) and select View credentials.

3. Copy and paste just the url from the credentials to the url field of the vcap-local.json file, replacing CLOUDANT_DATABASE_URL.

4. Run your application locally.
```
$ npm start
```
View your local app at http://localhost:3000. Any names you enter into the app will now get added to the database.

*Avoid trouble:* IBM Cloud defines the PORT environment variable when your app runs on the cloud. When you run your app locally, the PORT variable is not defined, so 3000 is used as the port number. See Run your app locally for more information.

Your local app and the IBM Cloud app are sharing the database. Names you add from either app will appear in both when you refresh the browsers.

## Next steps:
1. https://cloud.ibm.com/docs/solution-tutorials?topic=solution-tutorials-tutorials
2. https://ibm-cloud.github.io/
3. https://www.ibm.com/cloud/architecture/architectures
