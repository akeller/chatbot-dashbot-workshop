# Conversation Sample Application With Dashbot

This Node.js app demonstrates the Conversation service in a simple chat interface simulating ordering a simple pizza with Dashbot Analytics.

## Before you begin

* Create an IBM Cloud account
    * [Sign up](https://console.ng.bluemix.net/registration/?target=/catalog/%3fcategory=watson) in Bluemix, or use an existing account. Your account must have available space for at least 1 app and 1 service.
* Make sure that you have the following prerequisites installed:
    * The [Node.js](https://nodejs.org/#download) runtime, including the [npm][npm_link] package manager

## Installing locally

If you want to modify the app or use it as a basis for building your own app, install it locally. You can then deploy your modified version of the app to the IBM Cloud.

### Getting the files

Use GitHub to clone the repository locally, or [download the .zip file](https://github.com/watson-developer-cloud/conversation-simple/archive/master.zip) of the repository and extract the files.

### Setting up the Conversation service

Create an IBM Cloud account if you haven't already.

Click "Catalog" in the upper menu. This will take you to the list of available services, platforms, and other offerings on the IBM Cloud. To make it simple, use the column menu and click "Watson" at the very bottom. This will filter the catalog to just show Watson Services.

![alt text][WatsonCatalogOfferings]

Click on Conversation to start the provisioning of the service.

Name the service, and select a region/location to deploy in. If applicable, choose an organization and space or leave it to the default if you only have one of each.

![alt text][conversation-top]

For pricing plans, keep the Lite plan, but notice your limits.

![alt text][conversation-bottom]

Click "Create" and wait for the service to provision. This may take some time.

Once the service is provisioned successfully, click on the Launch Tool button. This opens the Watson Conversation tool and may require you to login again.

![alt text][conversation-manage]

The page that loads contains your workspaces. Click the icon to import a workspace.

![alt text][workspaces]

Import the [json file](https://github.com/IBM/watson-conversation-slots-intro/blob/master/data/watson-pizzeria.json) from the [Pizza Ordering Code Pattern](https://developer.ibm.com/code/patterns/assemble-a-pizza-ordering-chatbot-dialog/). Choose the file and import everything. This will create a new workspace with all the intents, entities, and dialog intact.

Click the new workspace tile to open it.

![alt text][Workspace-dashboard]

Click the Deploy icon.

Click Credentials (next to Deploy Options). Copy your workspace ID, username and password. Enter these as strings in the C# script to access the API.

![alt text][credentials]

...

Alternatively, from the IBM Cloud Conversation page (page where the Launch tool button sits), click "Service credentials" then expand "View credentials" to reveal your username and password. Enter these as strings in the C# script to access the API. This does not show your workspace ID.

![alt text][conversation-service-credentials]



### Importing the Conversation workspace [optional]

1. In your browser, navigate to [your IBM Cloud console] (https://console.ng.bluemix.net/dashboard/services).

1. From the **All Items** tab, click the newly created Conversation service in the **Services** list.

    ![Screen capture of Services list](readme_images/conversation_service.png)

1. On the Service Details page, click **Launch tool**.

1. Click the **Import workspace** icon in the Conversation service tool. Specify the location of the workspace JSON file in your local copy of the app project:

    `watson_pizza.json`

1. Select **Everything (Intents, Entities, and Dialog)** and then click **Import**. The car dashboard workspace is created.

### Importing from Bot Asset Exchange (BAE) [optional]

1. In your browser, navigate to the [Bot Asset Exchange] ().

1. Choose a bot you want to use and click download.

1. Login with your IBM Cloud account information.

1. ...

### Configuring the app environment (via bash)

1. Copy or rename the `.env.example` file to `.env` (nothing before the dot).

1. Create a service key in the format `cf create-service-key <service_instance> <service_key>`. For example:

    ```bash
    cf create-service-key my-conversation-service myKey
    ```

1. Retrieve the credentials from the service key using the command `cf service-key <service_instance> <service_key>`. For example:

    ```bash
    cf service-key my-conversation-service myKey
    ```

   The output from this command is a JSON object, as in this example:

    ```JSON
    {
      "password": "87iT7aqpvU7l",
      "url": "https://gateway.watsonplatform.net/conversation/api",
      "username": "ca2905e6-7b5d-4408-9192-e4d54d83e604"
    }
    ```

1. Paste  the `password` and `username` values (without quotation marks) from the JSON into the `CONVERSATION_PASSWORD` and `CONVERSATION_USERNAME` variables in the `.env` file. For example:

    ```
    CONVERSATION_USERNAME=ca2905e6-7b5d-4408-9192-e4d54d83e604
    CONVERSATION_PASSWORD=87iT7aqpvU7l
    ```

1. In your IBM Cloud console, open the Conversation service instance where you imported the workspace.

1. Click the menu icon in the upper-right corner of the workspace tile, and then select **View details**.

    ![Screen capture of workspace tile menu](readme_images/workspace_details.png)

1. Click the ![Copy](readme_images/copy_icon.png) icon to copy the workspace ID to the clipboard.

1. On the local system, paste the workspace ID into the WORKSPACE_ID variable in the `.env` file. Save and close the file.

### Installing and starting the app (via bash)

1. Install the demo app package into the local Node.js runtime environment:

    ```bash
    npm install
    ```

1. Start the app:

    ```bash
    npm start
    ```

1. Point your browser to http://localhost:3000 to try out the app.

## Testing the app

After your app is installed and running, experiment with it to see how it responds.

For more information about intents, see the [Conversation service documentation][doc_intents].

To see details of how these intents are defined, including sample input for each intent, launch the Conversation tool.

## Modifying the app

After you have the app deployed and running, you can explore the source files and make changes. Try the following:

* Modify the .js files to change the app logic.
* Modify the .html file to change the appearance of the app page.
* Use the Conversation tool to train the service for new intents, or to modify the dialog flow. For more information, see the [Conversation service documentation][docs_landing].

## Deploying to IBM Cloud

You can use Cloud Foundry to deploy your local version of the app to IBM Cloud.

1. In the project root directory, open the `manifest.yml` file:

  * In the `applications` section of the `manifest.yml` file, change the `name` value to a unique name for your version of the demo app.
  * In the `services` section, specify the name of the Conversation service instance you created for the demo app. If you do not remember the service name, use the `cf services` command to list all services you have created.

  The following example shows a modified `manifest.yml` file:

  ```yml
  ---
  declared-services:
   conversation-service:
     label: conversation
     plan: free
  applications:
  - name: conversation-simple-app-test1
   command: npm start
   path: .
   memory: 256M
   instances: 1
   services:
   - my-conversation-service
   env:
     NPM_CONFIG_PRODUCTION: false
  ```

1. Push the app to IBM Cloud:

  ```bash
  cf push
  ```
  Access your app on IBM Cloud at the URL specified in the command output.

## Troubleshooting

If you encounter a problem, you can check the logs for more information. To see the logs, run the `cf logs` command:

```none
cf logs <application-name> --recent
```

## License

This sample code is licensed under Apache 2.0.
Full license text is available in [LICENSE](LICENSE).

## Contributing

See [CONTRIBUTING](CONTRIBUTING.md).

## Open Source @ IBM

Find more open source projects on the
[IBM Github Page](http://ibm.github.io/).


[cf_docs]: (https://console.bluemix.net/docs/services/watson/getting-started-cf.html)
[cloud_foundry]: https://github.com/cloudfoundry/cli#downloads
[demo_url]: http://conversation-simple.ng.bluemix.net/
[doc_intents]: (https://console.bluemix.net/docs/services/conversation/intents-entities.html#planning-your-entities)
[docs]: https://console.bluemix.net/docs/services/conversation/index.html
[docs_landing]: (https://console.bluemix.net/docs/services/conversation/index.html)
[node_link]: (http://nodejs.org/)
[npm_link]: (https://www.npmjs.com/)
[sign_up]: bluemix.net/registration
[conversation-top]: ./readme_images/conversation-top.png "alt text"
[conversation-bottom]: ./readme_images/conversation-bottom.png "alt text"
[conversation-manage]: ./readme_images/conversation-manage.png "alt text"
[conversation-service-credentials]: ./readme_images/conversation-top.png "alt text"
[workspaces]: ./readme_images/workspaces.png "alt text"
[Workspace-dashboard]: ./readme_images/Workspace-dashboard.png "alt text"
[credentials]: ./readme_images/credentials.png "alt text"
[WatsonCatalogOfferings]: ./readme_images/WatsonCatalogOfferings.png "alt text"
