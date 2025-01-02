# Expense Tracker

This repository showcases a serverless expense management system built using AWS services. The project demonstrates expertise in AWS architecture, integrating key services such as Lambda, API Gateway, DynamoDB, S3, Cognito, and SNS.

---

## Table of Contents
1. [Overview](#overview)
2. [AWS Services Used](#aws-services-used)
3. [System Architecture](#system-architecture)
4. [Lambda Functions](#lambda-functions)
5. [How to Use the APIs](#how-to-use-the-apis)
6. [Sample Receipts](#sample-receipts)
7. [Project Flow](#project-flow)
8. [Future Enhancements](#future-enhancements)

---

## Overview
This project enables users to upload receipt files, process them to extract expense details, and store the data in DynamoDB. It showcases best practices in AWS serverless architecture, security via Cognito authentication, and robust error handling using CloudWatch logs.

---

## AWS Services Used
- **Amazon S3**: To store receipt files.
- **Amazon DynamoDB**: To store expense details.
- **AWS Lambda**: To process receipts, manage database operations, and send notifications.
- **Amazon Cognito**: For user authentication and API security.
- **Amazon API Gateway**: To expose Lambda functions as APIs.
- **Amazon CloudWatch**: For logging and monitoring Lambda functions.
- **Amazon SNS**: To send notifications on receipt upload.
- **IAM Roles and Policies**: To securely grant necessary permissions.

---

## System Architecture
The system is designed with the following flow:
1. User logs in via Cognito to obtain a JWT token for API authentication.
2. Receipt files are uploaded to S3 using an API endpoint.
3. S3 triggers a Lambda function via event notifications to process the file.
4. Receipt data is extracted and stored in DynamoDB.
5. Notifications are sent via SNS on successful file uploads.
6. Users can add or retrieve expense details directly via API endpoints.

Refer to the below Flowchart for a detailed view to understand the system's flow, highlighting the interaction between AWS services, APIs, and users
![AWSv1](https://github.com/user-attachments/assets/a57cd41c-a5ef-46a1-96fa-644fdbe56c35)


---

## Lambda Functions
The repository includes the following Lambda functions:

1. **Upload & Process Receipts**:
   - Validates the JWT token to authenticate the user.
   - Accepts a base64-encoded receipt file and uploads it to S3 with metadata.
   - Extracts date, total amount, and transaction ID from the receipt.
   - Stores the data in DynamoDB.

2. **Sends Email Notification**:
   - Sends Email Notification to the user on successful receipt upload

3. **Add Expense**:
   - Manually adds expense details to DynamoDB.

4. **Get Expense**:
   - Fetches expense details from DynamoDB by user.

---

## How to Use the APIs
1. **Prerequisites**:
   - Install [Postman](https://www.postman.com/downloads/). 
   - Use Content-Type application/x-www-form-urlencoded while retrieving the JWT token from Cognito's /oauth2/token API endpoint
   - Get the Client ID and Client Secret from Cognito and encode it in base64 with the format \<Client ID>:\<Client Secret>
   - Pass this base64 encoded to the /oauth2/token API endpoint of Cognito in the Authorization header as Basic \<base64 encoded data>
   - Register your account and log in using your email and password in Cognito's Hosted UI
   - Get the authorization_code from the hosted ui's url
   - Get a JWT token (id_token) by sending that authorization_code to the same API endpoint
   - Use that id_token/JWT token as the Bearer token in the header of all APIs that you'll invoke in this project (Currently the token is valid for 24 hrs)

2. **API Endpoints**:
   - **Upload Receipt**:
     - Method: `POST`
     - URL: `<API-Gateway-URL>/upload-receipt`
     - Headers:
       - `Authorization`: Bearer `<JWT Token>`
     - Body (JSON):
       ```json
       {
         "file": "<base64-encoded-file>",
         "fileName": "receipt1.txt"
       }
       ```
   - **Add Expense**:
     - Method: `POST`
     - URL: `<API-Gateway-URL>/expenses`
     - Headers:
       - `Authorization`: Bearer `<JWT Token>`
     - Body (JSON):
       ```json
       {
         "ExpenseId": "12345",
         "UserId": "user123",
         "Amount": 50.75,
         "Category": "Food",
         "Timestamp": "2025-01-01"
       }
       ```
   - **Get Expense**:
     - Method: `GET`
     - URL: `<API-Gateway-URL>/expenses`
     - Headers:
       - `Authorization`: Bearer `<JWT Token>`
     - Body (JSON): Retrieves all expenses for a particular user id
       ```json
       {
         "UserId": "user123",
       }
       ```
     - Body (JSON): Retrieves only that specific expense for a particular user id
       ```json
       {
         "UserId": "user123",
         "ExpenseId": "12345"
       }
       ```

3. **Testing**:
   - Use the provided sample receipts in the `Receipts` folder.

---

## Sample Receipts
The repository includes two sample receipt files in the `Receipts` folder. You can use these to test the system by converting the contents to base64 and uploading via Postman.

---

## Future Enhancements
- Add frontend for better user interaction.
- Implement advanced receipt data extraction using AI/ML.
- Enhance error notifications using AWS Step Functions.

---

## License
This project is licensed under the MIT License. See the LICENSE file for details.

---

For any queries or feedback, please open an issue or reach out!

