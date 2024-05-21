# API Destination Challenge
### Challenge goal
Process all events for source com.aws.orders via an Amazon EventBridge API Destination. In this use case we are demonstrating how you might integrate EventBridge with third party public API endpoints using an outbound webhook with Basic Auth configured.

## Step 1: Identify the API URL
1. Open the AWS Management Console for CloudFormation . You can find the API URL for this challenge in the Outputs of the CloudFormation Stack with a name containing ApiUrl.

API URL in CloudFormation

## Step 2: Configure the EventBridge API Destination with basic auth security
Open the AWS Management Console for EventBridge  in a new tab or window, so you can keep this step-by-step guide open.

On the EventBridge homepage, select API destinations from the left navigation.

On the API destinations page, select Create API destination.

Select API destinations

On the Create API destination page

Enter api-destination as the Name
Enter the API URL identified in Step 1 as the API destination endpoint
Select POST as the HTTP method
Select Create a new connection for the Connection
Enter basic-auth-connection as the Connection name
Select Basic (Username/Password) as the Authorization type
Enter myUsername as the Username
Enter myPassword as the Password
Create API destination

Click Create

Step 3: Configure an EventBridge rule to target the EventBridge API Destination
From the left-hand menu, select Rules.

From the Event bus dropdown, select the Orders event bus.

Click Create rule

On the Define rule detail page

Enter OrdersEventsRule as the Name of the rule
Enter Send com.aws.orders source events to API Destination for Description
Under Build event pattern

Choose Other for your Event source
Copy and paste the following into the Event pattern, and select Next to specify your target:
1
2
3
4
5
{
    "source": [
        "com.aws.orders"
    ]
}

Create Bus

Take a moment and study the event pattern that you pasted. The event pattern allows you to subscribe to only events that has a source from com.aws.orders.
Select your rule target:

Select EventBridge API destination as the target type.
Select api-destination from the list of existing API destinations
Create API destination target

Click Next and finish walking through the rest of the walk-through to create the rule.

Step 4: Send test Orders event
Below is sample data to test your rule. If you don't remember how to use Send events function to send an event, please refer to the previous section.

Using the Send events function, send the following Order Notification events from the source com.aws.orders:

1
{ "category": "lab-supplies", "value": 415, "location": "us-east" }

Step 5: Verify API Destination
If the event sent to the Orders event bus matches the pattern in your rule, then the event will be sent to an API Gateway REST API endpoint.

Open the AWS Management Console for CloudWatch Log Groups  in a new tab or window, so you can keep this step-by-step guide open.

Select the Log group with an API-Gateway-Execution-Logs prefix.

Select Log group

Select the Log stream.

Select Log stream

Toggle the log event to verify the basic authorization was successful and the API responds with a 200 status.

Verify API request

Congratulations! You have successfully created your first custom event. We will now present additional challenges for you to complete. A description of the goal, sample data, and verification steps have been provided, but it is up to you to write the correct event pattern. Remember, use CloudWatch Logs to troubleshoot your rule implementation, if you are not able to verify your rule.