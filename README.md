[![Build Status](https://api.travis-ci.org/IBM/watson-voice-bot.svg?branch=master)](https://travis-ci.org/IBM/watson-voice-bot)

# Create a web based chatbot with voice input and output

In this code pattern we will create a web based chat bot, but the twist here is that we'll be using voice input and output. For the conversation dialog we'll of course be using Watson Assistant, but we'll also be using Watson Speech To Text to capture the user's voice, and lastly we'll use Watson Text To Speech to playback the chatbots response to the user. The web application itself is built on top of JQuery and Python Flask.

When the reader has completed this code pattern they will understand how to:

* Make a Watson Speech To Text call using a Web Socket Connection
* Make a Watson Text to Speech REST API call
* Send and receive messages to Watson Assistant using REST APIs
* Integrate Watson Speech To Text, Watson Text To Speech and Watson Assistant in a web app

![](doc/source/images/architecture.png)

## Flow

1. User selects the microphone option on the browser and speaks.
2. The voice is passed on to Watson Speech To Text using a Web Socket connection.
3. The text from Watson Speech to Text is extracted and sent as input to Watson Assistant.
4. The response from Watson Assistant is passed onto Watson Text to Speech.
5. The audio output is sent to the web application and played back to the user, while the UI also displays the same text.

## Included components

* [Watson Speech-to-Text](https://www.ibm.com/watson/services/speech-to-text/): A service that converts human voice into written text.
* [Watson Text-to-Speech](https://www.ibm.com/watson/services/text-to-speech/): Converts written text into natural sounding audio in a variety of languages and voices.
* [Watson Assistant](https://www.ibm.com/watson/ai-assistant/): Create a chatbot with a program that conducts a conversation via auditory or textual methods.

## Featured technologies

* [Flask](http://flask.pocoo.org/): Python is a programming language that lets you work more quickly and integrate your systems more effectively.
* [jQuery](https://jquery.com/): It is a cross-platform JavaScript library designed to simplify the client-side scripting of HTML.

<!--
# Watch the Video
[![](https://img.youtube.com/vi/Jxi7U7VOMYg/0.jpg)](https://www.youtube.com/watch?v=Jxi7U7VOMYg)
-->

## Steps

We'll have two methods to run this application: [using an easy one-click deploy option](#using-the-deploy-to-ibm-cloud-button), or [locally with a few CLI tools](#run-the-application-locally). Depending on your objective, learning or seeing the app quickly, choose accordingly.

## Using the `Deploy to IBM Cloud` button

> If you prefer to deploy the application manually, scroll down to the next section.

Steps:

1. [Clone the repo](#1-clone-the-repo)
2. [Create the services and deploy the web app](#2-create-the-services-and-deploy-the-web-app)
3. [Upload the Watson Assistant workspace](#3-upload-the-watson-assistant-workspace)
4. [Configure environment variables](#4-configure-environment-variables)

### 1. Clone the repo

Clone the `watson-voice-bot` repo locally. In a terminal, run:

```bash
git clone https://github.com/IBM/watson-voice-bot
```

We’ll be using the file [`data/workspace.json`](data/workspace.json).

### 2. Create the services and deploy the web app

The next step is to deploy the application, we'll do this by clicking the button below. A nice byproduct of doing this is that the services required (Speech-to-Text, Text-to-Speech, and Assistant) will be created automatically. Click the button below.

[![Deploy to IBM Cloud](https://cloud.ibm.com/devops/setup/deploy/button.png)](https://cloud.ibm.com/devops/setup/deploy?repository=https://github.com/NaderBadawy/WatsonVoice.git)

When the DevOps Toolchain Pipeline appears, click on the `Deploy` button to start the deployment. You can optionally, click on the `Delivery Pipeline` to watch the logs as the app is deployed.

![toolchain pipeline](doc/source/images/toolchain-pipeline.png)

Once deployed, the app can be viewed by clicking `View app`. The app can also be viewed in the dashboard. The app should be prefixed with the string `watson-voice-bot-`, and the corresponding services that were created can easily be identified by the `wvb-` prefix, i.e.: `wvb-assistant`.

### 3. Upload the Watson Assistant workspace

* Find the Assistant service in your IBM Cloud Dashboard.
* Click on the service and then click on `Launch tool`.
* Go to the `Skills` tab.
* Click `Create new`
* Click the `Import skill` tab.
* Click `Choose JSON file`, go to your cloned repo dir, and `Open` the workspace.json file in [`data/workspace.json`](data/workspace.json).
* Select `Everything` and click `Import`.

To find the `WORKSPACE_ID` for Watson Assistant:

* Go back to the `Skills` tab.
* Find the card for the workspace you would like to use. Look for `watson-online-store`.
* Click on the three dots in the upper right-hand corner of the card and select `View API Details`.
* Copy the `Workspace ID` GUID. Save it for the .env file

!["Get Workspace ID"](https://github.com/IBM/pattern-utils/blob/master/watson-assistant/assistantPostSkillGetID.gif)

Optionally, to view the conversation dialog select the workspace and choose the **Dialog** tab. Here's a snippet of the dialog:

![dialog](doc/source/images/dialog.png)

### 4. Configure environment variables

The last step to perform is to configure our application to use the right Watson Assistant dialog. We'll solve this by specifying the `workspace ID` as an environment variable that the web application has access to read. To do this we'll navigate to our application overview from the [IBM Cloud Dashboard](https://cloud.ibm.com/dashboard/apps/), searching for `watson-voice-bot` and clicking on the name.

From the application overview, we can click on the `Runtime` menu located in the navigation bar on the left. Select the `Environment Variables` tab. Scroll down and click on `Add`.

![add env variable](doc/source/images/add_env_variable.png)

We'll add the environment variable key name as `WORKSPACEID` and for the value we'll use the workspace ID from Step 3. Click `Save` and wait for the application to reload.

Once the application restarts, click on the generated URL and start interacting with your voice enabled chatbot! See [Sample output](#sample-output) for things to say to your chatbot.

## Run the application locally

1. [Clone the repo](#1-clone-the-repo)
2. [Create Watson services with IBM Cloud](#2-create-watson-services-with-ibm-cloud)
3. [Upload the Watson Assistant workspace](#3-upload-the-watson-assistant-workspace)
4. [Configure `.env` with credentials](#4-configure-env-with-credentials)

### 1. Clone the repo

Clone the `watson-voice-bot` repo locally. In a terminal, run:

```
git clone https://github.com/IBM/watson-voice-bot
```

We’ll be using the file [`data/workspace.json`](data/workspace.json).

### 2. Create Watson services with IBM Cloud

Create the following services:

* [**Watson Conversation**](https://cloud.ibm.com/catalog/services/conversation)
* [**Watson Speech To Text**](https://cloud.ibm.com/catalog/services/speech-to-text)
* [**Watson Text To Speech**](https://cloud.ibm.com/catalog/services/text-to-speech)

### 3. Upload the Watson Assistant workspace

* Find the Assistant service in your IBM Cloud Dashboard.
* Click on the service and then click on `Launch tool`.
* Go to the `Skills` tab.
* Click `Create new`
* Click the `Import skill` tab.
* Click `Choose JSON file`, go to your cloned repo dir, and `Open` the workspace.json file in [`data/workspace.json`](data/workspace.json).
* Select `Everything` and click `Import`.

To find the `WORKSPACE_ID` for Watson Assistant:

* Go back to the `Skills` tab.
* Find the card for the workspace you would like to use. Look for `watson-online-store`.
* Click on the three dots in the upper right-hand corner of the card and select `View API Details`.
* Copy the `Workspace ID` GUID. Save it for the .env file

!["Get Workspace ID"](https://github.com/IBM/pattern-utils/blob/master/watson-assistant/assistantPostSkillGetID.gif)

Optionally, to view the conversation dialog select the workspace and choose the **Dialog** tab. Here's a snippet of the dialog:

![dialog](doc/source/images/dialog.png)

### 4. Configure `.env` with credentials

Our services are created and workspace uploaded. It's now time to let our application run locally and to do that we'll configure a simple text file with the values we want to use. We begin by copying the [`sample.env`](sample.env) file and naming it `.env`.

```
cp sample.env .env
```

We now populate the key-value pairs with credentials for each IBM Cloud service (Assistant, Speech To Text, and Text To Speech). These values can be found in the `Services` menu in IBM Cloud, by selecting the `Service Credentials` option for each service.

Lastly, the `WORKSPACEID` value was retrieved in the previous step, we use that value here.

#### `sample.env`:

```
# Replace the credentials here with your own.
# Rename this file to .env before starting the app.

# Watson Assistant
WORKSPACE_ID=<add_assistant_workspace>
ASSISTANT_URL=<add_assistant_url>
ASSISTANT_IAM_APIKEY=<add_assistant_apikey>

## Un-comment to use username+password for an older instance, and comment out ASSISTANT_IAM_APIKEY
# ASSISTANT_USERNAME=<add_assistant_username>
# ASSISTANT_PASSWORD=<add_assistant_password>

# Watson Speech To Text
SPEECHTOTEXT_USER=<add_stt_username>
SPEECHTOTEXT_PASSWORD=<add stt password>

# Watson Text To SpeechToText
TEXTTOSPEECH_USER=<add tts username>
TEXTTOSPEECH_PASSWORD=<add tts password>
```

### 5. Run the application

* The general recommendation for Python development is to use a virtual environment [(venv)](https://docs.python.org/3/tutorial/venv.html). To install and initialize a virtual environment, use the `venv` module on Python 3 (you install the virtualenv library for Python 2.7):


Create the virtual environment using Python. Use one of the two commands depending on your Python version.
> Note: it may be named python3 on your system.

```bash
python -m venv mytestenv       # Python 3.X
virtualenv mytestenv           # Python 2.X
```

Now source the virtual environment. Use one of the two commands depending on your OS.

```bash
source mytestenv/bin/activate  # Mac or Linux
./mytestenv/Scripts/activate   # Windows PowerShell
```
> **TIP** :bulb: To terminate the virtual environment use the `deactivate` command.

1. Start the app by running:

```bash
pip install -r requirements.txt
python welcome.py
```

2. Launch a browser and navigate to [http://localhost:5000](http://localhost:5000)
3. Click on the microphone icon to begin speaking and click it again when you are finished.

# Sample output

Follow the script below for a sample path in an insurance domain, or configure your own dialog under Watson Assistant.

```
Voice Bot: Hello. How can I help you?
User Reply: I am trying to buy an insurance

Voice Bot: You have come to the right place We offer you a smart, affordable and convenient way to safeguard the future for you and your family. Can I ask you a couple of questions to come up with the best plan for you?
User Reply: ok sure

Voice Bot: Please tell me your Name followed by Date Of Birth and Annual Income
User Reply: This is Mary

Voice Bot: Please enter your Date of Birth
User Reply: 2nd June 1945

Voice Bot: Your annual income?
User Reply: 1200000 dollars

Voice Bot: Do you smoke?
User Reply: Nope I dont

Voice Bot: Please tell me your gender
User Reply: Female

Voice Bot: Enter a protection type
Select one among Basic Life Cover, Fixed Income Protection or Increasing Income Protection.
User Reply: Life Cover

Voice Bot: What premium payment frequency would you prefer? Monthly, Quarterly, Half Yearly or Yearly?
User Reply: Monthly

Voice Bot: What term would you want to go for? Enter a range between 10 years to 35 years.
User Reply: 8 years

Voice Bot: Please enter a term ranging between 10 years to 35 years.
User Reply: 12

Voice Bot: Your yearly premium for 12 years under the Basic Life Cover plan is 67767 dollars. Do you want to try any other options or go ahead with buying this?
User Reply: No What is Basic Life Cover?

Voice Bot: On death during the Policy term, Sum Assured as a lump sum is paid to the nominee and the policy terminates.
User Reply: ok Thanks

Voice Bot: Have a great day
```

![sample output](doc/source/images/sample_output.png)

## Troubleshooting

* If you are deploying to IBM Cloud and you get a `Deployment Error`:

```
Creating route watson-voice-bot.eu-gb.mybluemix.net...
OK

FAILED
Server error, status code: 400, error code: 210003, message: The host is taken: watson-voice-bot
```

Fix this by changing the name and host to something unique

https://github.com/IBM/watson-voice-bot/blob/23c5116031978bbc69c61d24f6f10394f1b9ec93/manifest.yml#L17
https://github.com/IBM/watson-voice-bot/blob/23c5116031978bbc69c61d24f6f10394f1b9ec93/manifest.yml#L18

## Links

* [Watson Node.js SDK](https://github.com/watson-developer-cloud/node-sdk)
* [Relevancy Training Demo Video](https://www.youtube.com/watch?v=8BiuQKPQZJk)
* [Relevancy Training Demo Notebook](https://github.com/akmnua/relevancy_passage_bww)

## Learn more

* **Artificial Intelligence Code Patterns**: Enjoyed this Code Pattern? Check out our other [AI Code Patterns](https://developer.ibm.com/technologies/artificial-intelligence/).
* **AI and Data Code Pattern Playlist**: Bookmark our [playlist](https://www.youtube.com/playlist?list=PLzUbsvIyrNfknNewObx5N7uGZ5FKH0Fde) with all of our Code Pattern videos
* **With Watson**: Want to take your Watson app to the next level? Looking to utilize Watson Brand assets? [Join the With Watson program](https://www.ibm.com/watson/with-watson/) to leverage exclusive brand, marketing, and tech resources to amplify and accelerate your Watson embedded commercial solution.

## License
This code pattern is licensed under the Apache Software License, Version 2.  Separate third party code objects invoked within this code pattern are licensed by their respective providers pursuant to their own separate licenses. Contributions are subject to the [Developer Certificate of Origin, Version 1.1 (DCO)](https://developercertificate.org/) and the [Apache Software License, Version 2](https://www.apache.org/licenses/LICENSE-2.0.txt).

[Apache Software License (ASL) FAQ](https://www.apache.org/foundation/license-faq.html#WhatDoesItMEAN)
