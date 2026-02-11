# Invoice Digitalization System

## Disclaimers
Customers are responsible for making their own independent assessment of the information in this document.

This document:

(a) is for informational purposes only,

(b) references AWS product offerings and practices, which are subject to change without notice,

(c) does not create any commitments or assurances from AWS and its affiliates, suppliers or licensors. AWS products or services are provided "as is" without warranties, representations, or conditions of any kind, whether express or implied. The responsibilities and liabilities of AWS to its customers are controlled by AWS agreements, and this document is not part of, nor does it modify, any agreement between AWS and its customers, and

(d) is not to be considered a recommendation or viewpoint of AWS.

Additionally, you are solely responsible for testing, security and optimizing all code and assets on GitHub repo, and all such code and assets should be considered:

(a) as-is and without warranties or representations of any kind,

(b) not suitable for production environments, or on production or other critical data, and

(c) to include shortcuts in order to support rapid prototyping such as, but not limited to, relaxed authentication and authorization and a lack of strict adherence to security best practices.

All work produced is open source. More information can be found in the GitHub repo.

### Description:

The InvoDigitize Suite is a comprehensive cloud-based solution designed to streamline the process of invoice management and digitization. Leveraging the power of AWS services, including Amplify, DynamoDB, Lambda, S3, Quicksight, Amazon Bedrock and AWS Texextractor, it offers a dual-component system that simplifies both the creation and analysis of digital invoices. From automated data entry to insightful business intelligence, the system transforms invoice handling for efficiency and scalability.

- Invoice Form: https://main.ddnx95rmp8ck9.amplifyapp.com/
- Invoice uploader: https://main.d8he5p2xkb6ya.amplifyapp.com/

### Overview:

The Invoice Digitalization System consists of two main components:

1. **Invoice Form Web Application**: Developed with Amplify and React JS, this component allows users to enter invoice data through a user-friendly web interface. Upon submission, the data is stored in a DynamoDB table named 'invoice', which triggers a Lambda function for JSON transformation and storage in an S3 bucket. This data integration allows for real-time updates and analytics in Quicksight, providing users with valuable insights from their invoice data.

2. **S3 Invoice Uploader**: This tool addresses the challenge of digitizing physical invoice copies. Users can upload scanned invoices via a simple interface, and these documents are stored into the S3 bucket. A triggered Lambda function processes these uploads using AWS Text Extractor, Amazon Bedrock: Claude v2 model to accurately capture and categorize invoice information. The processed data is then entered into the same 'invoice' DynamoDB table, enabling unified analysis in Quicksight alongside data from the web application.

The Invoice Digitalization System is designed for businesses seeking to modernize their invoice management processes. By automating data entry and leveraging cloud-based analytics, it offers a scalable, efficient solution for managing invoices from creation to insight generation.

Based on the attached high-level architecture diagram, here is a description that outlines the flow and components of your invoice digitization system:

### High-Level Architecture Description:

![Screenshot](hla.png)

**1. Invoice Upload and Extraction:**

- **Existing Invoice Documents**: Hard copy invoices are digitized and uploaded as PDFs.
- **Amazon S3**: The PDFs are stored in an S3 bucket, which serves as the initial repository for incoming invoice documents.
- **Amazon Textract**: Triggered by the addition of new files to the S3 bucket, Amazon Textract extracts text and data from the uploaded invoice PDFs.
- **Lambda Function**: This function is invoked post-uploading to perform any extraction, processing and transformation of the invoice pdfs.
- **Amazon Bedrock**: Further data processing and structuring are handled by Amazon Bedrock, ensuring the data matches the required format for the invoice database.

**2. Digital Invoice Entry:**

- **Data Entry Operators**: Users responsible for manual data entry interact with the system through a web interface.
- **Amazon Cognito**: Authenticates and authorizes data entry operators to ensure secure access to the system.
- **AWS Amplify**: Provides the framework for building the web application used for entering new invoices digitally.
- **Amazon DynamoDB**: Stores digital invoice data entered through the web application, acting as the central data repository for all invoice data.

**3. Data Processing to Analytics:**

- **Lambda Function**: Once data is in DynamoDB, a Lambda function formats this data into JSON, which is suitable for analytics.
- **Amazon S3**: The processed JSON data is then stored in a separate S3 bucket.
- **Amazon Quicksight**: Utilizes the JSON data from the S3 bucket to perform analytics, offering business intelligence and insights to stakeholders.

**4. Business Insights:**

- **Business Stakeholders**: End-users such as business analysts, finance teams, and decision-makers who access the insights and analytics generated by Amazon Quicksight to make informed decisions.

The architecture showcases a robust, cloud-based solution that efficiently processes and analyzes invoice data, providing a seamless flow from physical document to actionable business insights.

Certainly! Below is a basic outline for the Prerequisites, Deployment Guide, and User Guide for your InvoDigitize Suite system. You can expand on each section with more detailed steps based on your specific implementation and requirements.

## Prerequisites

Before deploying the system, ensure the following conditions are met:

- aws cdk needs to be installed
- Access to AWS Bedrock models that needs to be configured from aws console
- AWS Command Line Interface installed and configured with your credentials.
- Proper IAM roles and policies in place for Lambda, DynamoDB, S3, Textract, Bedrock, Cognito, Amplify, and Quicksight.
- Set up a Cognito User Pool for managing user authentication.
- Node.js, npm (or yarn), and React installed for web application development.

## Deployment Guide

### Backend Services:

1. **DynamoDB Setup**:

   - Create a new DynamoDB table named 'invoice'.
   - Define the primary key and any secondary indexes based on your data model.

2. **S3 Buckets**:

   - Create two S3 buckets: one for storing uploaded invoice PDFs and another for the processed JSON files.

3. **Lambda Functions**:

   - Deploy Lambda functions for processing data: Textract extraction and DynamoDB entry.
   - Ensure Lambda has the necessary execution roles.

4. **Amazon Textract and Bedrock**:

   - Configure Amazon Textract to work with your lambda function.
   - Set up Bedrock for your data processing needs.

5. **Quicksight**:
   - Sign up for Quicksight and set up a dataset connected to your S3 bucket with JSON data.
   - Design the necessary analysis and dashboards.

### Frontend Application:

1. **Amplify Initialization**:

   - Run `amplify init` in your project directory to initialize a new Amplify project.

2. **Cognito Integration**:

   - Use Amplify to add authentication to your app with `amplify add auth`.
   - Push the changes with `amplify push`.

3. **Deploying Web Application**:
   - Develop your React application for invoice data entry.
   - Use `amplify add hosting` to set up hosting and deploy your app with `amplify publish`.
   - Or Connect the github repository and deploy it in the Amplify app

### Testing:

- Perform a test run by uploading a PDF invoice to the S3 bucket and ensure that all the triggers and processes work as expected.

## User Guide

### Accessing the System:

1. **Logging In**:
   - Navigate to the web application URL.
   - Use your credentials to log in via the Cognito authentication process.

### Using the Invoice Form:

1. **Entering Invoice Data**:
   - Fill out the invoice form with the necessary information.
   - Submit the form to send the data to the DynamoDB table.

### Uploading Invoice PDFs:

1. **Uploading Documents**:
   - Use the provided interface to upload scanned invoice PDFs to the designated S3 bucket.
   - Monitor the upload status and confirm successful uploads.

### Viewing Analytics:

1. **Accessing Quicksight**:
   - Log in to the AWS Quicksight console.
   - Navigate to your dashboard to view analytics and insights derived from your invoice data.

## Credits

Developer: Chaitrali Shinde

Architect: Arun Arunachalam

General Manager, ASU: Ryan Hendrix

This project is designed and developed with guidance and support from the ASU Cloud Innovation Center.
