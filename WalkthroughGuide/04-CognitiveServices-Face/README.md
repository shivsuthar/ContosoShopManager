![Banner](Assets/Banner.png)

# Face Services Overview

Azure Computer Vision for Face offers comprehensive capabilities to deal with photos with faces.

You can visit [Face Portal](https://azure.microsoft.com/en-us/services/cognitive-services/face/) to test and evaluate the different capabilities.

## Azure Backend Setup

You need to provision a dedicated Cognitive Service for Face APIs. This can be done easily by heading out to [Azure Portal](https://portal.azure.com) and click **Create New Service** then select Computer Vision - Face:

![face-azure](Assets/face-azure.png)

After successfully provisioning the service, take a note of both the endpoint and subscription key as you will need them when accessing the Face APIs.

![face-azure-overview](Assets/face-azure-overview.png)

## Face Authentication

Part of Cognitive Services Face APIs is [Face Verify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a), which verify whether two faces belong to a same person or whether one face belongs to a person.

Contoso Biometric Authentication uses Face Verify to authenticate a live capture face image against a predefined list of employee faces.

In order to achieve this scenario, some preparation work needs to be done. Face APIs have capabilities to store faces in a secure data store.

Faces data store is hosted under what is called a [Person Group](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244) or [Large Person Group](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d). A person group is the container of the uploaded person data, including face images and face recognition features. After creation, use [PersonGroup Person - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c) to add persons into the group, and then call [PersonGroup - Train](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249) to get this group ready for [Face - Identify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239).

>***NOTE:*** The different between **Person Group** and **Large Person Group** is size. Large Person Group can hold up to 1,000,000 people while Person Group can handle 10,000 (under S0-tier subscription)

## Face APIs - Postman

To try out the different Face APIs preparation work, this workshop includes a ***Postman*** collection extract that can be imported. Collection is organized in folders that each include the relevant operations for each part of the Face APIs usage process.

![Postman Face APIs](Assets/face-postman.png)

Also you need to import the [Dev](../../Src/Postman-APIs/Dev.postman_environment.json) environment variables that are being used through out the APIs.

>**NOTE:** All APIs Postman collections used through out this workshop can be found under [Src/Postman-APIs](../../Src/Postman-APIs)

Steps to leverage Face Authentication scenario include:

1. Create a Person Group (consider this as the organization or department, default ID used is 1)
2. Create a Person (inside the created Person Group (represent an individual inside the organization)
3. Create a Person Face (with a URL to the image that hold face of the person. You can add multiple faces to a person for better recognition)
4. Train the system on the newly formed Person Group (you must do this every time you add or update Person in the Person Group)
5. Verify the training status to ensure that Face APIs are using up to date model.
6. Now you can start verifying faces using Face Detect and Verify

## Face Explorer

Included with this workshop a nice Angular web application that provides GUI to interact with the Face APIs from setup to verification.

You can access the source code for [Face Explorer here](../../Src/FaceExplorer-App).

![Face Explorer App](Assets/face-explorer-app.png)

If you wish to run the Angular app locally on you machine, you need to have [Angular CLI](https://cli.angular.io/) installed. Below are the steps you should perform:

1. [NodeJs](https://nodejs.org/en/) must be installed.
2. You can then use NPM to install Angular CLI using the following command:

```js
npm install -g @angular/cli
```

>***NOTE:*** You can now open the project inside Visual Studio Code to perform the next steps. Once it is opened, you can launch a new terminal window like the screen shot below:
![Visual Studio Code](Assets/face-explorer-terminal.png)

3. Next step will be installing all project dependencies using NPM (must be run inside the [FaceExplorer-App](../../Src/FaceExplorer-App) folder):

```js
npm install
```

4. You can then use command line or Visual Studio Code to build and run the Angular project (command must be executed in folder [FaceExplorer-App](../../Src/FaceExplorer-App)).

```js
ng serve
```

![ng serve](Assets/face-explorer-ng-serve.png)

5. Copy the URL from the terminal and pasted in the browser.
6. Add your endpoint and subscription key to Face API service from Azure to the Face Explorer Service Configuration here [FaceExplorer-App/src/app/services/face-api-service.service.ts](../../Src/FaceExplorer-App/src/app/services/face-api-service.service.ts)

***Endpoint (line 11):***

```js
private baseUrl = '<specify Face API base URL here>';
```

***Subscription Key (line 130):***

```js
const httpOptions = {
  headers: new HttpHeaders({
    'Content-Type': 'application/json',
    'Ocp-Apim-Subscription-Key': '<specify Face API key here>'
  })
};
```

7. Save the changes and refresh your browser and everything should be set to go.

# Next Steps

[Shelves Compliant AI Model](../05-CognitiveServices-CustomVision)