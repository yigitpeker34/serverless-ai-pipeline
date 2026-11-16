# Serverless AI Pipeline on AWS

An event-driven, serverless pipeline that analyzes customer reviews with AI: it detects language and sentiment, raises an email alert for negative feedback, and makes the results queryable and visible on a dashboard.

Built as a project for the **Cloud Computing** course of the M.Sc. Professional IT Business & Digitalization at **HTW Berlin** (Winter Semester 2025/26).

## Architecture

![Architecture diagram](YIGIT_PEKER_CLOUD_PRJ.png)

All resources run in the AWS region **eu-west-1 (Ireland)**.

## How it works

| Stage | What happens |
|---|---|
| CI/CD | GitHub Actions automatically deploys the backend code to the Lambda function and the frontend code to the website S3 bucket. |
| Ingestion | A text file with customer reviews (one review per line) is uploaded to the raw-data S3 bucket. |
| Trigger | The upload event automatically invokes the Lambda function. |
| AI analysis | Lambda processes the file line by line. For each review, Amazon Translate detects the source language and translates the text to English, then Amazon Comprehend runs sentiment analysis on the English text. |
| Storage | Lambda writes one JSON object per review (JSON Lines) with original text, detected language, English translation, sentiment and confidence score to the processed-data S3 bucket, so Athena can read each row. |
| Analytics | An AWS Glue crawler scans the processed bucket and updates the schema, so the data can be queried with SQL in Amazon Athena. |
| Visualization | An Amazon QuickSight dashboard is embedded in a static website hosted on S3. |

The Lambda function runs under a dedicated IAM role that gives it access to S3 and the AI services.

## AWS services used

| Service | Role in the project |
|---|---|
| Amazon S3 | Raw data, processed data and static website hosting |
| AWS Lambda | Event-driven processing logic |
| Amazon Comprehend | Sentiment analysis |
| Amazon Translate | Automatic language detection and translation to English |
| Amazon SNS | Email alerts for negative feedback |
| AWS Glue | Crawler that maintains the data schema |
| Amazon Athena | SQL queries on the processed data |
| Amazon QuickSight | Dashboard, embedded in the website |
| AWS IAM | Execution role for the Lambda function |
| GitHub Actions | Automated deployment (CI/CD) |

## Author

Yigit Peker, HTW Berlin
