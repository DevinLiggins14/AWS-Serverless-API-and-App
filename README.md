# AWS-Serverless-API-and-App
<h2>Description</h2>
<br/> In this project we will host a static web app, create a rest API, use serverless computing, authenticate users, store data within a NoSQL DB, translate URL, and improve performance via caching
<br />
<br/> Project Architecture: <br/>
<img src="https://github.com/user-attachments/assets/e4201958-eb72-4d2e-9f54-91836f0037ea"/>

<img src="https://github.com/user-attachments/assets/ac10e086-d864-4026-9626-887595fe7999"/>
<h2> Services involved: </h2>

| **Service**    | **Purpose**                                                                                     | **Key Benefits**                                                                                     |
|-----------------|-----------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **S3**          | Host the static web app and store its files.                                                   | No server setup required, highly scalable, and accessible to any number of users at any time.       |
| **API Gateway** | Create a RESTful API with different paths and HTTP methods for the web app to connect to.       | Simplifies API creation, management, and integration with other AWS services.                       |
| **Lambda**      | Execute code whenever the API Gateway receives a request.                                       | Fully serverless, auto-scales, and runs code on demand.                                             |
| **DynamoDB**    | Store and retrieve data for the app using a NoSQL database.                                     | No database server management required, scalable, and reliable.                                     |
| **Cognito**     | Manage user authentication and authorization.                                                  | Provides user sign-up, sign-in, and secure access to APIs and other resources.                      |
| **Route53**     | (Optional) Configure a custom domain for the web app hosted in S3.                             | Simplifies domain management and provides seamless integration with AWS services.                   |
| **CloudFront**  | Improve performance by caching static files from S3 across global edge locations.              | Ensures low-latency access to static files by delivering content from servers closest to the user.   |

---

### **Notes for Usage**
1. **Required Services**: S3, API Gateway, Lambda, DynamoDB, Cognito.
2. **Optional Services**: Route53 (for custom domains), CloudFront (for faster performance).  



<p align="center">
  
### **Prerequisites**  
- Have an [AWS account](https://aws.amazon.com/console/)   

 ##  Step 1: Create a New API


<br/> In order to connect our web app to an API to store data we will need a restful API. For this we will use API Gateway which will make it easy for us to create an API with different paths and HTTP methods <br/>
 <br/> Navigate to API Gateway and create a rest API <br/>

 <img src="https://github.com/user-attachments/assets/11d8c3ae-5767-4509-bf4f-700a5880172e"/>

  <br/> Add resources <br/>

 <img src="https://github.com/user-attachments/assets/fc2510af-873e-47a0-97cc-9f892c5461cb"/>
 
<br/> Setting up Cross-Origin Resource Sharing (CORS) for your API is crucial because it allows your application to specify which domains can interact with your API resources, ensuring secure cross-origin requests. Without proper CORS configuration, browsers may block requests from different origins, which can disrupt the functionality of web applications relying on your API. <br/>

  <br/> Verify the correct headers <br/>

 <img src="https://github.com/user-attachments/assets/ead6c0cb-a95c-42db-ace2-d6c52eba8597"/>

  <br/> Create a POST request that will trigger a Lambda Function <br/>

 <img src="https://github.com/user-attachments/assets/13b13dc1-1fb8-404d-989c-730b4dd0d004"/>

  <br/> But wait in order to complete this request method we must have a Lambda function created <br/>

  ## Step 2: Create a Lambda Function

<br/> We will use AWS Lambda and connect it to our API Gateway to execute code whenever we receive a request to one of our custom endpoints 
 <br/>

 <br/> Navigate to Lambda Functions and create  <br/>
 <img src="https://github.com/user-attachments/assets/71162284-cec5-4643-bb92-05d9a9a3cb79"/>

  <br/> Remove async and add the following arguements <br/>

 <img src="https://github.com/user-attachments/assets/5e6fe862-2bdd-4be4-8d09-46990aeaecb6"/>

  <br/> Add the following Nodejs code. The exports.handler syntax defines the function as a CommonJS module, making it available for AWS Lambda to invoke. This handler processes the event and context inputs, then uses the callback to send a JSON response.  <br/>

```Bash
exports.handler = (event, context, callback) => {
  // TODO implement
  callback(null, { message: "Hi, I'm Lambda!" });
};
```

 <img src="https://github.com/user-attachments/assets/65d36dae-67e5-4a2e-9542-184ac04836f8"/>


## Step 3: Connect the Lambda Function to API Gateway endpoints 
  <br/> Now that the function has been created we can return to the API and connect. (create a new stage and deploy the API after changes) <br/>

 <img src="https://github.com/user-attachments/assets/89b81a38-091c-44af-b71a-c3f275c89cbf"/>
<img src="https://github.com/user-attachments/assets/7f2e56ef-f849-409e-af18-96e13fa89cdf"/>
<br/> The API Gateway is now visible as the trigger in the Lambda Function we created <br/>
<img src="https://github.com/user-attachments/assets/62035d02-773f-47f7-8039-a647e676d5d6"/>

  <br/> Now let's test the API <br/>

 <img src="https://github.com/user-attachments/assets/a6c6fa57-c0c5-4cb6-9196-c4703428095d"/>

  <br/> We've successfully connected our Lambda Function to our API Gateway. Now we can test our new POST request by entering the invoke url into POSTMAN (Don't forget the invoke url will be different if a new stage was created) <br/>

 
 <img src="https://github.com/user-attachments/assets/2347024e-03a8-418d-bfc4-75b6fbf668a4"/>

<br/> Make sure to include the stage and resource in the URL. That's all we need at the moment no API key or Auth Token is required due to cors * <br/>

<br/> Let's go back to our Lambda Function and change the code show the event object  <br/> 

```Bash
exports.handler = (event, context, callback) => {
  // TODO implement
  callback(null, event);
};
```

<img src="https://github.com/user-attachments/assets/40de24bf-c44a-437a-88ba-21b46e0b4bcc"/>

<br/> Now go back to the API Gateway and test to send the following request body <br/>

```
{
    "name": "Devin",
    "age": 300
}
```

<img src="https://github.com/user-attachments/assets/4db56015-a17c-4937-976e-945cd62d1133"/>

<br/> Click test to see we get exactly the same output as the reponse body. We could change this by configuring the integration request and integration response <br/>

<img src="https://github.com/user-attachments/assets/932647cf-d253-451b-ae61-163c4ec289c0"/>

## Step 4: Enable Lambda Proxy integration 
  
  <br/> Configure the API to use Lambda Proxy integration so complete request data will be passed and not just the request body   <br/>


 <img src="https://github.com/user-attachments/assets/a9f40115-4ec0-44ce-aa63-c04245c7db76"/>
<img src="https://github.com/user-attachments/assets/cbe36dfa-c714-4de4-aec1-7891c7a57d95"/>

  <br/> Now add the following code to the Lambda configuration <br/>

```Bash
exports.fn = (event, context, callback) => {
  // Log the event to CloudWatch
  console.log("Received event:", JSON.stringify(event, null, 2));

  // Return a response
  callback(null, {
    statusCode: 200,
    headers: {
      'Access-Control-Allow-Origin': '*'
    },
    body: JSON.stringify({})
  });
};
```

 <img src="https://github.com/user-attachments/assets/e2b380d0-020d-499c-ae11-5d30043bba93"/>

<br/> This code defines an AWS Lambda function that logs the incoming event to CloudWatch Logs for debugging or tracking purposes. It then responds to the client with an HTTP status code 200, a CORS header (Access-Control-Allow-Origin: *), and an empty JSON body. This setup ensures the function can handle cross-origin requests while providing a simple, functional response. <br/>

  <br/> We can now test it from our API Gateway and head over to CloudWatch to view the logs  <br/>

 <img src="https://github.com/user-attachments/assets/cb665cf0-1421-4538-8e28-377e5e6819cd"/>
  <img src="https://github.com/user-attachments/assets/2c473721-f479-4da4-bb07-c2e0389698a0"/>
<img src="https://github.com/user-attachments/assets/88c66f22-b884-4bd9-8e0f-573c6c681ae3"/>
  
  <br/> In order for our Lambda Function to only return relevant event data lets configure the following: 
  Turn off Lambda Proxy integration that we configured earlier, 
  <br/>

 <img src=""/>

  <br/> <br/>

 <img src=""/>

  <br/> <br/>

 <img src=""/>

  <br/> <br/>

 <img src=""/>

  <br/> <br/>

 <img src=""/>

  <br/> <br/>

 <img src=""/>

 <br/> <br/>

 <img src=""/>

 <br/> <br/>

 <img src=""/>

 <br/> <br/>

 <img src=""/>

 <br/> <br/>

 <img src=""/>

 <br/> <br/>

 <img src=""/>

 <br/> <br/>

 <img src=""/>

 <br/> <br/>

 <img src=""/>

 <br/> <br/>

 <img src=""/>

 <br/> <br/>

 <img src=""/>

 <br/> <br/>

 <img src=""/>

 <br/> <br/>

 <img src=""/>

 <br/> <br/>

 <img src=""/>
