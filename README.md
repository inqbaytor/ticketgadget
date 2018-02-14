# ticketgadget Light Weight REST API

## Preconditions
* You have obtained a valid API key from http://inqbaytor.io (or sandbox)
* You understand the booking creation requirements of your preferred GDS. Please refer to your GDS provider's website for more information
* Your callback endpoint is ready to receive the API response.

## Getting Started
* Request for an API key from http://inqbaytor.io
* Once we get the request, our team will review the request and provide you with a unique "AppKey" for your app.
* You need to use this key in the header, for us to be able to authorize your requests😀

## So, How can ticketgadget API issue your agency tickets?



## Issue ticket using PNR
Since our system is GDS independent, you can just send the GDS and the PNR/URL/UR code to issue the ticket. Yes, Its that simple 👍.

Once the user has inserted the GSD and PNR/URL/UR code, your backend services should trigger the following request to our API:

**Endpoint:**  
https://sanbox.inqbaytor.io/v7/app/hit

**Method:**  
POST

**Header parameters:**

| **Parameter [Type]** | **Required** | **Description**         | **Example**                  |
| -------------------  | ------------ | ----------------------- | ---------------------------- |
| AppKey [String]      | yes          |  Application secret key | a0fa6814cb9040a5e887e7115d66 |

**Body parameters:**

| **Parameter [Type]** | **Required** | **Description**                                                     | **Example**              |
| -------------------  | ------------ | ------------------------------------------------------------------- | ------------------------ |
| PNR/URL/UR[bookingID]   | yes          | Booking identifire                                                 | VUZC3P                   |
| GDS [String]       | no           | Optional parameter. Fallback is set to Travelport 1G | TRV-1G |
| ticketstates [String]       | no           | Optional parameter that will be send back to partner's callbak url. | NO PLATING CARRIER QUOTA |

**Initiate User Authorization**  
```bash
curl -X POST -H "Content-Type: application/json" -H "appKey: a0fa6814cb9040a5e887e7115d66" -H "Cache-Control: no-cache" -d '{
  "PNR": VUZC3P
}' "https://sanbox.inqbaytor.io/v7/app/hit"
```

The **"GDS"** param is an optional param that you might wanna send to the API if you are using multiple GDS's, once the user triggers the authorization with ticketgadget API. The same param will be routed back to you along with the ticketstates to your callback endpoint, which will help you complete the ticketing transaction.

Alternatively, you can create and keep a server-side session, using the **PNR** you get in the response.

**Response**

A successful request (200 response code), means your request is accepted.

In the response, you'll get the corresponding PNR and the ticketstates (if set to yes) back.

```json
{
  "VUZC3P" : "PNR": "NO PLATING CARRIER QUOTA"
}
```

In case of a failed request, the response codes in return are:

- 403 Forbidden - **means the SDK login can't be used for that number (non-existing account)**
  {
  "code": 1008,
  "message": "Forbidden: It is forbidden to use sdk login for this PNR."
  }
  
- 404 Not Found - **means your credentials (AppKey) are not valid.**
  {
  "code": 404,
  "message": "Invalid partner credentials."
  }

- 5xx Server error - **any other undefined error**

Note: Once the user triggers the authorization from your app, it would be good to lock the behaviour for a certain period of time (5 mins). The reason is to prevent multiple unnecessary requests in a short period of time towards our platform, and allowing proper completion of the cycle.

**Method:**  
All requests will be submitted as a POST request. Make sure your service is async, since we only expect that the message is accepted from your side. The service should respond within maximum 2000 Milliseconds upon receiving the request.

**Security:**  
To ensure security and privacy, HTTPS should be used. Make sure your certificate is always valid.
