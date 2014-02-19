# How to Run and Deploy the Node MySQL Upload App #

## Overview of the app ##

This is a NodeJS app that uses the following cloud services:

-   MySQL Database

This app demonstrates how to connect to a MySQL database on BlueMix from a NodeJS app. 
Simply upload a line-separated file of text (e.g. tweets), and it will add each line to MySQL.

## License ##
Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with the License. You may obtain a copy of the License at

     http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.

## Prerequisites ##

Before we begin, we first need to install the command line tool that will be used to upload and manage your application. Cloud Foundry uses a tool called [**cf**](https://github.com/cloudfoundry/cli).  Make sure you are using v6 of the cf cli by using

	cf -v
	
to see the version.

## Deploying the App and Binding the MySQL Service ##

In the terminal, go to the directory of the app, and follow these steps.

1. Login to Bluemix.
 * `$ cf login -a https://api.ng.bluemix.net`
2. Create an instance of the postgreSQL service, giving it a unique name in the last arguement.
 * `$ cf create-service mysql 100 unique_service_instance_name`
3. From the directory that houses the *app.js* file, push the app with a -c flag to start the node app and a --no-start option so we can bind our required service before starting our app.  Give your app a unique app name to be used as its path.
 * `$ cf push unique_app_name --no-manifest --no-start -c="node app.js"`
4. Bind the MySQL service instance to the new app
 * `$ cf bind-service unique_app_name unique_service_instance_name`
5. Start the app
 * `$ cf start unique_app_name`

*Note* : that you must add the flag **-c="node app.js"** in order for the app to start correctly.

*Note* : Service instance names must be unique to your organization.

As an example invocation... 

 * `$ cf login -a https://api.ng.bluemix.net`
 * `$ cf create-service mysql 100 mysql_NMU`
 * `$ cf push nmu --no-manifest --no-start -c="node app.js"`
 * `$ cf bind-service nmu mysql_NMU`
 * `$ cf start nmu`

*Note* : `-c="node app.js"` assumes you have not changed the filename for the Node.js app.


## Troubleshooting ##
-   Sometimes your app may not work as expected and debugging needs to be done. The cf command line tool can be used to assist with debugging. With the cf you can check your app's logs by typing the command **cf logs [app_name]** 
-   When you first start using the cf tool, you may potentially have trouble logging in due to no target being set. To view the target that is set, type **cf target** and if you want to set a new target type **cf target [target_url]**. Note: The target URL will usually be in the form of http://api.xxx.tld
-   From time to time your app may stop working, this means it could require a restart. To do this you must first stop it by typing **cf stop**. Once the app has been stopped, you can type **cf start** and if there are no other problems your app should start. 
