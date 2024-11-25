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

 ##  Step 1: 

