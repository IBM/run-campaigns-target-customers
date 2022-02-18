# Determine target audience and run marketing campaigns 

A business runs marketing campaigns to promote products with the objective of boosting revenues. The campaigns need to be run on appropriate audiences for maximum impact. A consumer not interested in a product will ignore the campaign offer.

There are two steps to running a marketing campaign:

* Identifying the target audience - The target audience can be determined by analyzing the purchases and browsing history of customers, social media posts, reviews and other data sources. This will help identify customers who could be interested in a product.  

* Run campaigns(e-mail, sms, phone etc.) on target audience.


![](images/usecase_flow.png)

In this pattern, the following aspects are covered:
* Identifying the target audience to run a campaign in a certain product category.
* Run an E-mail campaign for the target audience.

The customer demographics and sales data available [here](https://dataplatform.ibm.com/exchange/public/entry/view/f8ccaf607372882403a37d9019b3abf4) is used to demonstrate the above. The sales data includes multiple categories of products. For this pattern, we demonstrate identifying the target audience and running a campaign for *Canned Foods*.

When the reader has completed this pattern, they will understand how to:
* Create and run a Jupyter notebook in Watson Studio.
* Use Object Storage to access data files.
* Use Python Pandas to derive insights and come up with a target audience based on the customer purchase data.
* Integrate with Watson Campaign Automation to run a campaign on the target audience.

The intended audience for this code pattern are Developers and Data Scientists who want to build an end to end marketing campaign solution for a business.

![](images/architecture.png)


1. The Object storage stores the data.
2. Data is utilized as csv files.
3. The Jupyter notebook processes the data and generates the target audience.
4. The Jupyter notebook is powered by Spark.
5. The target audience information is sent to Watson campaign automation(WCA) to run campaigns.

## Included components

* [IBM Watson Studio](https://www.ibm.com/cloud/watson-studio): Analyze data using RStudio, Jupyter, and Python in a configured, collaborative environment that includes IBM value-adds, such as managed Spark.

* [IBM Cloud Object Storage](https://console.bluemix.net/catalog/infrastructure/cloud-object-storage): An IBM Cloud service that provides an unstructured cloud data store to build and deliver cost effective apps and services with high reliability and fast speed to market.

* [Watson campaign automation](https://www.ibm.com/in-en/marketplace/digital-marketing-and-lead-management): Smarter marketing automation across all digital channels, powered by Watson.

## Featured technologies

* [Data Science](https://medium.com/ibm-data-science-experience/): Systems and scientific methods to analyze structured and unstructured data in order to extract knowledge and insights.

* [Marketing automation](https://en.wikipedia.org/wiki/Marketing_automation): Software platforms and technologies designed for marketing departments and organizations to more effectively market on multiple channels online (such as email, social media, websites, etc.) and automate repetitive tasks.

# Watch the Video

[![](http://img.youtube.com/vi/_Nk4vjHsxfI/0.jpg)](https://youtu.be/_Nk4vjHsxfI)

# Steps

Follow these steps to setup and run this developer journey. The steps are
described in detail below.

1. [Sign up for Watson Studio](#1-sign-up-for-watson-studio)
1. [Create contacts database in Watson Campaign Automation](#2-create-contacts-database-in-watson-campaign-automation)
1. [Create campaign contact list in Watson Campaign Automation](#3-create-campaign-contact-list-in-watson-campaign-automation)
1. [Create campaign mailing template in Watson Campaign Automation](#4-create-campaign-mailing-template-in-watson-campaign-automation)
1. [Configure application access in Watson Campaign Automation](#5-configure-application-access-in-watson-campaign-automation)
1. [Create the notebook](#6-create-the-notebook)
1. [Add the data](#7-add-the-data)
1. [Update the notebook with service credentials](#8-update-the-notebook-with-service-credentials)
1. [Update Watson Campaign Automation URLs and credentials in the notebook](#9-update-watson-campaign-automation-urls-and-credentials-in-the-notebook)
1. [Run the notebook](#9-run-the-notebook)
1. [Analyze the results](#10-analyze-the-results)

## 1. Sign up for Watson Studio

Sign up for IBM's [Watson Studio](https://datascience.ibm.com/). 

## 2. Create contacts database in Watson Campaign Automation

Login to the [Watson campaign automation](https://www.ibm.com/in-en/marketplace/digital-marketing-and-lead-management) instance.

Select `Data` and then `Databases`. Select `Shared` and click `Create`.

![](images/create_database.png)

Configure the database by entering a name `wdp_wca_db`. Click on `Next`.

![](images/configure_database.png)

Click on `Create` to create the database.

![](images/create_db_finish.png)

Select `Data` and then `Databases`. Click `View` to view the list of databases.

![](images/goto_contact_db.png)

Click on the database `wdp_wca_db`.

![](images/select_contact_db.png)

Note the database id.

![](images/note_database_id.png)


## 3. Create campaign contact list in Watson Campaign Automation

Select `Data` and then `Contact Lists`. Click on `Create Contact List`.

![](images/view_contact_list.png)

Configure the campaign list by selecting the parent database as `wdp_wca_db`. Enter a name `campaign_canned_food_contact_list`. Click `OK`.

![](images/configure_campaign_list.png)

Select `Data` and then `Contact Lists`. Click `View` to view the list of contact lists.

![](images/goto_contact_list.png)

Click on the list `campaign_canned_food_contact_list`.

![](images/select_contact_list.png)

Note the contact list id.

![](images/note_contact_list_id.png)

## 4. Create campaign mailing template in Watson Campaign Automation

Click on `Content`. Under `Create Mailings` click on `Mailing Template`.

![](images/invoke_create_mailing_template.png)

Select the `Welcome Simple` mailing template.

![](images/select_mailing_template.png)

Click to open the selected mailing template. Click `Open` to customize the mailing template.

![](images/open_mailing_template.png)

Click on `Select Contacts` and select `campaign_canned_food_contact_list`. Click `Done`.

![](images/customize_mailing_template.png)

Customize the template as shown below and change the name to `CannedFoodsCampaignJune2018`. Click on `Save`.

![](images/edit_name_save_template.png)

Click on `Content`. Under `View Mailings` click on `Templates`.

![](images/view_mailing_templates.png)

The list of templates can be seen. Hover the mouse over `CannedFoodsCampaignJune2018` and note the template id.

![](images/list_templates.png)

## 5. Configure application access in Watson Campaign Automation

Login to the Watson Campaign Automation instance. Go to `Settings` and select `Organization Settings`.

![](images/invoke_settings.png)

Goto `Application Account Access` and click `Add Application`.

![](images/invoke_add_application.png)

Add an application name `customer_insights_wstudio` and click `Add`.

![](images/add_application_name.png)

Note the client id and client secret. Click `Close`.

![](images/note_client_secret.png)

Click on `Add Account Access`.

![](images/invoke_account_access.png)

Select the application `customer_insights_wstudio` and click `Add`.

![](images/enter_account_access_details.png)

A refresh token is sent to the registered email-d. Please note the refresh token.

## 6. Create the notebook
* In [Watson Studio](https://dataplatform.ibm.com) - create a project if necessary, provisioning an object storage service if required.
* Go to `Tools` and select `Notebook`.
* Select the `From URL` tab.
* Enter a name for the notebook.
* Optionally, enter a description for the notebook.
* Select the project.
* Enter this Notebook URL: https://github.com/IBM/run-campaigns-target-customers/blob/master/notebooks/campaign_management.ipynb
* Select the runtime as shown below.
* Click the `Create` button.

![](images/create_notebook_from_url.png)

## 7. Add the data

#### Add the data to the notebook

* Please download the file from https://dataplatform.ibm.com/exchange/public/entry/view/f8ccaf607372882403a37d9019b3abf4.
* Rename the file as `customer_orders.csv`
* All the email addresses in the file are invalid. For testing the pattern, modify and add valid email address for couple of contacts that get added to Watson Campaign Automation after the notebook execution.
* From your project page in Watson Studio, click `Find and Add Data` (look for the `10/01` icon)
and its `Files` tab.
* Click `browse` and navigate to where you downloaded `customer_orders.csv` on your computer.
* Add the files to Object storage.

![](images/add_file.png)

## 8. Update the notebook with service credentials 

#### Add the Object Storage credentials to the notebook

* Select the cell below `2.1 Add your service credentials for Object Storage` section in the notebook to update the credentials for Object Store.
* Use `Find and Add Data` (look for the `10/01` icon) and its `Files` tab. You should see the file names uploaded earlier. Make sure your active cell is the empty one created earlier.
* Select `Insert to code` below `customer_orders.csv`.
* Click `Insert Crendentials` from the drop down menu.
* If the credentials are written as `credential_2` change them to `credentials_1`.

![](images/objectstorage_credentials.png)

## 9. Update Watson Campaign Automation URLs and credentials in the notebook

#### Enter the access token generation URL,client id, client secret and refresh token in section 5.

Replace the `<BASE_URL>` with the base url for the Watson Campaign Automation instance.
Add the client id, client secret and refresh token noted in section 5.

![](images/clientid_secret_token.png)

#### Enter the url, database id, contact list id and template id noted in sections 2,3 and 4

Replace the `<BASE_URL>` with the base url for the Watson Campaign Automation instance. The `<BASE_URL>` specified here can be different from the `<BASE_URL>` entered previously for getting access tokens.

Enter the database id , contact list id and template id we noted in sections 2,3 and 4.

![](images/xmlapiurl_dbid.png)

## 9. Run the notebook

When a notebook is executed, what is actually happening is that each code cell in
the notebook is executed, in order, from top to bottom.

Each code cell is selectable and is preceded by a tag in the left margin. The tag
format is `In [x]:`. Depending on the state of the notebook, the `x` can be:

* A `blank`, this indicates that the cell has never been executed.
* A `number`, this number represents the relative order this code step was executed.
* A `*`, this indicates that the cell is currently executing.

There are several ways to execute the code cells in your notebook:

* One cell at a time.
  * Select the cell, and then press the `Play` button in the toolbar.
* Batch mode, in sequential order.
  * From the `Cell` menu bar, there are several options available. For example, you
    can `Run All` cells in your notebook, or you can `Run All Below`, that will
    start executing from the first cell under the currently selected cell, and then
    continue executing all cells that follow.
* At a scheduled time.
  * Press the `Schedule` button located in the top right section of your notebook
    panel. Here you can schedule your notebook to be executed once at some future
    time, or repeatedly at your specified interval.

For this Notebook, you can simply `Run All` cells.

## 10. Analyze the results

Once the notebook execution is complete, an email is sent to all the contacts in the contact list `campaign_canned_food_contact_list`.

![](images/campaign_mail.png)

## Troubleshooting

[See DEBUGGING.md.](DEBUGGING.md)

## License

This code pattern is licensed under the Apache Software License, Version 2.  Separate third party code objects invoked within this code pattern are licensed by their respective providers pursuant to their own separate licenses. Contributions are subject to the [Developer Certificate of Origin, Version 1.1 (DCO)](https://developercertificate.org/) and the [Apache Software License, Version 2](http://www.apache.org/licenses/LICENSE-2.0.txt).

[Apache Software License (ASL) FAQ](http://www.apache.org/foundation/license-faq.html#WhatDoesItMEAN)