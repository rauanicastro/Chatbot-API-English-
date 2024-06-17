## Chatbot API

By using our Chatbot API, you can send WhatsApp text messages to your customers in a fast and easy way, and also receive status updates of your sent messages.

### Getting Started

To use our API successfully, first you will need to authenticate yourself.

Perform a basic authentication by providing your username and password in the header. If you are not registered and do not have an username or password yet, [sign up on the Blandy Global Platform](https://blandyglobalfakewebsite.com).

The path you will use to make API calls is https://api.blandy.chatbot.com/v1.

```bash
curl -u "username:password" https://api.blandy.chatbot.com/v1
```
### Creating User

You need to create an user in order to get your own unique registration user ID. To create your user, call `POST /user/` and fill the request body with your information.

```JSON
curl -X 'POST' \
'https://api.blandy.chatbot.com/v1/user/' \
-H 'accept: application/json' \
-H 'Content-Type: application/json' \
-u "Fake.CX:123456",
{
"username": "Fake.CX", 
"email": "fake.cx@test.com", 
"phone": "+558199990000" 
}
```

The `201 Created` response indicates that the user was created and it returns a JSON informing your `unique_id`.

```JSON
{
"unique_id": "00f0e000-0c00-000a-0000-0000a0000a0c", 
"status": "available"
}
```

### Sending Messages

Call `POST /messages/` to send text messages to your customers via WhatsApp, entering the following JSON objects in the request body:

 - **unique_id**: unique registration user ID
 - **from**: message sender
 - **to**: message recipient
 - **text**: message to send

```JSON
curl -X 'POST' \ 
'https://api.blandy.chatbot.com/v1/messages/' \ 
-H 'accept: application/json' \ 
-H 'Content-Type: application/json' \ 
-u "Fake.CX:123456", 
{
"metadata": {
	"unique_id": "00f0e000-0c00-000a-0000-0000a0000a0c",
},
    "from": "Fake.CX",
    "to": "558399998888",
    "text": "Message Test Sample"
}
```

After a successful request with a `200` response, the API returns the `request_id` and the `url_callback` so you can receive message status updates.

```JSON
{
"request_id": "adb0000a-000d-0d00-ab00-00b0b0d0e0fe",
"url_callback: "https://studiob-qa.blandy.dev/message/webhook"
}
```

The possible status updates that you can receive are described in the table below:

| Name | Description |
:---: | :---: |
| sent | The message was sent and is currently in transit within the system|
| delivered | The message was delivered to your customer |
| read | The message was read by your customer *(`read` notifications are only available for customers that have enabled read receipts)*|
| failed | The message failed to be sent *(check our [Troubleshooting Documentation](https://blandyglobalfakewebsite.com/troubleshooting) to help you debug)* |
| deleted | The message was deleted by your customer |

### Error Responses

If any problem occurs during the  `POST /messages/` call, here are the error responses you can get:

**Code 400**

You will get this error response when your JSON body is malformed.

```JSON
{
"message": "Invalid JSON"
}
```

**Code 500**

This error indicates an unexpected issue on the server's side that prevented it from fulfilling the request.

```JSON
{
"message": "Internal Server Error"
}
```
