# ticketgadget Light Weight REST API

## Preconditions
* You have obtained a vaild API key from http://inqbaytor.io (or sanbox)
* You understand the booking creation requirments of your preffered GDS. Please reffer to your GDS providers website for more information
* Your callback endpoint is ready to receve the API responce.

## Getting Started
* Request for a API key from http://inqbaytor.io
* Once we get the request, our team will review the request and provide you with a unique "AppKey" for your app.
* You need need to use this key in the header, for us to be able to authorize your requests😀

## So, How can ticketgadget API issue your agency tickets?



## Issue ticket using PNR
Since our system is GDS indipendent, you can just send the GDS and the PNR/URL/UR code to issue the ticket. Yes, Its that simple 👍.

Once the user has inserted the GSD and PNR/URL/UR code, your backend services should trigger the following request to our API:

**Endpoint:**  
https://sanbox.inqbaytor.io/v7/app/hit

**Method:**  
POST

**Header parameters:**

| **Parameter [Type]** | **Required** | **Description**         | **Example**                  |
| -------------------  | ------------ | ----------------------- | ---------------------------- |
| appKey [String]      | yes          |  Application secret key | a0fa6814cb9040a5e887e7115d66 |

**Body parameters:**

| **Parameter [Type]** | **Required** | **Description**                                                     | **Example**              |
| -------------------  | ------------ | ------------------------------------------------------------------- | ------------------------ |
| PNR/URL/UR[booking]   | yes          | Booking identifire                                                 | VUZC3P                   |
| GDS [String]       | no           | Optional parameter. Fallback is set to Travelport 1G | TRV-1G |
| ticket states [String]       | no           | Optional parameter that will be send back to partner's callbak url. | NO PLATING CARRIER QUOTA |
