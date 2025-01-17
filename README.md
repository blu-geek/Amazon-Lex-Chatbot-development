# Developing a Virtual Bot Assistant that helps with Booking Car Hailing Services and Hotels

# Architecture

<img width="799" alt="Screenshot 2025-01-17 at 11 35 23" src="https://github.com/user-attachments/assets/f1c1db24-e10d-4090-af82-94787707255c" />


# Activity Guide: Developing a Chatbot Assistant for Car Hailing and Hotel Booking Services Using Amazon Lex and Amazon Connect

1. Set Up the AWS Environment (IAM, Amazon Lex, Amazon Connect, and Amazon S3)

- Create IAM Roles (IAM):
- Create a role with the following permissions:
AmazonLexFullAccess for Lex.
AWSLambdaExecute for Lambda integration.
AmazonConnectFullAccess for Connect.
Name it LexConnectRole.
- Enable Amazon Lex and Connect:
Activate both services in your AWS region.
- Create S3 Bucket (Amazon S3):
Set up a bucket (e.g., lex-connect-logs) for storing call logs and bot interactions.

2. Create an Amazon Connect Instance (Amazon Connect)

- Access Amazon Connect:
Navigate to the Amazon Connect Console and click Create an Instance.
- Configure Access:
Set up administrative access and a user account for yourself.
- Claim a Phone Number:
Choose a phone number (toll-free or direct) for inbound and outbound calls.
- Enable Contact Control Panel (CCP):
Use CCP for managing calls via Amazon Connect.

3. Create a Lex Bot for Car Hailing and Hotel Booking (Amazon Lex)

- Create the Bot:
Name: CarHotelBookingBot.
Language: English (US).
- Design Intents:
- Car Hailing Intent:
Utterances: "Book a car", "Get me a taxi", etc.
Slots: PickupLocation, DropoffLocation, PickupTime.
- Hotel Booking Intent:
Utterances: "Book a hotel", "Find a room", etc.
Location, CheckInDate, CheckOutDate, Guests.
- Enable Fulfillment:
Choose Lambda function or No Code Action for processing bookings.

4. Fulfill Requests Using Lambda (Optional, AWS Lambda)

- Step 1. Write the Lambda Function:
- Example for hotel booking:
import json

def lambda_handler(event, context):
    intent_name = event['currentIntent']['name']
    if intent_name == 'HotelBookingIntent':
        location = event['currentIntent']['slots']['Location']
        check_in = event['currentIntent']['slots']['CheckInDate']
        check_out = event['currentIntent']['slots']['CheckOutDate']
        guests = event['currentIntent']['slots']['Guests']

        message = f"Your hotel in {location} from {check_in} to {check_out} for {guests} guests is confirmed."
        return {
            'dialogAction': {
                'type': 'Close',
                'fulfillmentState': 'Fulfilled',
                'message': {'contentType': 'PlainText', 'content': message}
            }
        }
- Step 2: Attach Lambda Function:
Add the function in Lex under Fulfillment Settings for each intent.

5. Fulfill Requests Without Lambda (Amazon Lex No-Code Setup)

- Step 1: Configure Slot Prompts:
- Set prompts like:
"Where would you like to book a car?"
"What is your check-in date for the hotel?"
- Step 2: Define Fulfillment Responses:
- Add static responses, such as:
"Your car booking from {PickupLocation} to {DropoffLocation} is confirmed."

6. Integrate Lex with Amazon Connect

- Access Connect Contact Flows:
In the Amazon Connect Console, go to Contact Flows > Create Contact Flow.
- Add Lex Integration:
Drag and drop the Get Customer Input block into the flow.
Configure it to use the CarHotelBookingBot.
- Add Call Routing Logic:
Add blocks to handle successful and unsuccessful bot interactions.
- Test the Integration:
Call the Connect phone number and verify the bot interaction.

7. Test the Chatbot

- Lex Console Test:
Use the built-in test tool to simulate user queries for both intents.
- Call Testing (Amazon Connect):
Call the Amazon Connect number and test the bot’s responses for car hailing and hotel booking.

8. Monitor and Optimize (CloudWatch and Lex Analytics)

- Monitor Logs:
Use CloudWatch Logs to track interaction details and debug issues.
- Review Analytics:
Analyze Lex reports to identify and improve user interactions.

9. Clean Up Resources

- Delete Lex Bot:
Remove the bot from the Amazon Lex Console.
- Remove Connect Instance:
Navigate to the Amazon Connect Console and delete the instance.
- Delete S3 Bucket:
Empty and delete the bucket used for logging interactions.
- Remove Lambda Function:
Delete the Lambda function if created.

10. Summary

Successfully developed and deployed a chatbot for car hailing and hotel booking services using Amazon Lex and Connect.
Integrated voice and text interactions with seamless call handling for enhanced user experience.
