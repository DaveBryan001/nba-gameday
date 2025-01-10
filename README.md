# NBA Game Updates Lambda Function

This project is a real-time alert system that keeps NBA fans updated on game-day scores and statuses through SMS or email notifications. Using AWS Lambda, Amazon SNS, and EventBridge, it fetches data from the NBA API and sends updates on scheduled, in-progress, or final games to subscribers. Built with Python, it’s a simple yet powerful way to demonstrate how cloud computing can create efficient and timely notification systems for sports enthusiasts.

## Project Structure

```
architecture-diagram.drawio
lambda_function.py
```

## Features

- Fetches live NBA game scores and updates from an external API, ensuring fans get the latest information.

- Sends well-formatted notifications to subscribers through Amazon SNS, keeping everyone in the loop.

- Automates the delivery of updates at set intervals using Amazon EventBridge, so you never miss a game update.

- Implements security best practices with least privilege IAM roles, ensuring the system is secure and reliable.


## Technologies Used

Cloud Provider: AWS

Core Services: SNS, Lambda, EventBridge

External API: NBA Game API (SportsData.io)

Programming Language: Python 3.13

IAM Security:

Policies with least privilege for Lambda, SNS, and EventBridge.


## **Architecture Diagram**
<img src="architecture-diagram.drawio.svg" alt="architecture" style="width:100%;"/>



## Setup Instructions

1. **Clone the Repository**
    ```bash
    git clone https://github.com/DaveBryan001/nba-gameday.git
    cd nba-gameday
    ```

2. **Create an SNS Topic**
    - Open the AWS Management Console and navigate to the SNS service.
    - Click **Create Topic**, select **Standard** as the topic type, and provide a name (e.g., `nba-topic`).
    - Note the ARN of the created topic for later use.

3. **Add Subscriptions to the SNS Topic**
    - Click on the topic name from the list, then go to the **Subscriptions** tab and click **Create subscription**.
    - Choose a protocol:
        - **Email**: Enter a valid email address.
        - **SMS**: Enter a valid phone number in international format (e.g., +1234567890).
    - Confirm the subscription:
        - For email, check your inbox and confirm via the provided link.
        - For SMS, the subscription becomes active immediately.

4. **Create the SNS Publish Policy**
    - In the IAM section of the AWS Console, go to **Policies** and click **Create Policy**.
    - Select the **JSON** tab and paste the contents of `nba-sns-policy.json`.
    - Replace placeholders for `REGION` and `ACCOUNT_ID` with your AWS details.
    - Name the policy (e.g., `nba-sns-policy`) and click **Create Policy**.

5. **Set Up IAM Role for Lambda**
    - In the IAM Console, go to **Roles** and click **Create Role**.
    - Select **AWS Service** and choose **Lambda**.
    - Attach the following policies:
        - `nba-sns-policy` (created in the previous step).
        - `AWSLambdaBasicExecutionRole` (AWS managed policy).
    - Name the role (e.g., `gd_role`) and create it.
    - Save the ARN of the role for Lambda configuration.

6. **Deploy the Lambda Function**
    - In the AWS Console, navigate to the Lambda service and click **Create Function**.
    - Select **Author from Scratch** and configure:
        - **Function Name**: `nba-notify`
        - **Runtime**: Python 3.x
        - **Role**: Use the IAM role created earlier.
    - Under **Function Code**:
        - Upload the `lambda_function.py` file or paste its content directly.
    - Add Environment Variables:
        - `NBA_API_KEY`: Your NBA API key.
        - `SNS_TOPIC_ARN`: The ARN of the SNS topic.
    - Save the function.

7. **Automate with EventBridge**
    - Navigate to the EventBridge service and click **Rules** → **Create Rule**.
    - Configure the rule:
        - **Event Source**: Schedule
        - **Cron Expression**: Set the desired update frequency (e.g., hourly).
    - Add a target and select the `lambda_function` Lambda function.
    - Save the rule.


## What i learned
- How to build a cloud-based notification system using AWS services.

- The importance of implementing secure and efficient workflows with IAM policies.

- Techniques for integrating external APIs into event-driven architectures.

- Automating processes effectively with Amazon EventBridge.




