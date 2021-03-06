# oauth2

This repository contains sample source codes for web applications want to authenticate their users using AUTHIN SSO™.

## Prerequisite

Relying party ( your application ) must be registered on AUTHIN platform first to get their credentials.

## Getting the tokens

User agent based application need to redirect the user to AUTHIN AccessManager™ and the the user will be redirected 
back to your application with tokens defining who is that user and what premissions they have.

### recieving tokens for the first time

#### Initializing the url

You should provide these parameters to `http://demo.authin.ir/openidauthorize` be successfully identified as a registered relying party (RP); these parameters are:

- client_id
- redirect_uri
- scope
- response_type
- nonce
- state ( optional )
- id_token_hint ( not required for first time )

##### client_id
 A string similar to `0a0f4bf3-bd3f-4169-8364-6a2b37e01372` that you get upon registration
 
##### redirect_uri
URL of the location your application expects to recieve its token

##### response_type
Requested token types; can be `token` which means request for access_token or `id_token` or both as `token id_token` which means you want both tokens.

##### scope
This parameter defines how much information you need from user, you can specify or just pass `openid` to get all possible information your application allowed to recieve about this user

##### nonce
A random string generated by your application to validate server response when recieving the tokens

##### state
A string you pass to OP ( OAuth Provider ) to be given back to your application after redirect. this could be anything, and is totally opaque to OP.

##### id_token_hint
Your previosly issued id_token.

### your final url would look like this
```bash
http://demo.authin.ir/openidauthorize?client_id=aa0s4bf3-bd3f-4169-8664-6a2b37e01372&redirect_uri=http://mywebsite.ir/token-recieving-page&scope=openid&response_type=token%20id_token&nonce=A_RANDOM_STRING&state=SOMETHING_YOU_WANT&id_token_hint=YOUR_PREVIOSLY_ISSUED_ID_TOKEN
```

## Recieving Tokens

After a successful login, the user will be redirected back to your redirect_uri with tokens and other requested information provided as queryParameters appended to url. the response would look like this: 

```bash
http://mywebsite.ir/token-recieving-page?scope=add_user read_posts profile email&access_token=eyJ0eXAiOiJKV1QiLCJhbGcipiJSUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzX3Rva2VuIiwiaXNzIjoiaHR0cHM6Ly93d3cuYXV0aGluLmlyIiwic3ViIjoiYWQ1YTU0MWUtYmQwNS00NjRjLWI3NzEtNWRlMmMyZGVlM2VlIiwiYXVkIjoiMGEwZjRiZjMtYmQzZi00MTY5LTgzNjQtNmEyYjM3ZTAxMzcyIiwiZXhwIjoxNTQ4NzkyODg3LCJpYXQiOjE1NDg3ODkyODcsInNjb3BlIjoidGizdCBwcm9maWxlIGVtYWlsIiwidHlwIjoiSldUIn0.dd5og-NeJq428fKnnzknoOuUxEK0COZXMKPYakeZgY7ByrgFBxTE_CFllPAX99CDX86ERy5nUBlfKf8vPK5gb9vJUdOeshkMIGfiJKwM0GJdx8NOC2kTDyYIsYOLubVASt4rWPwLt6RAvuWWohqp2Bx09gyrkQcZIAo1qRTwLrUKm7q71T_Ds-L4SLTs7P1Gw946DWY9LRLirK3dr-WEgrcxHAzf2Id841YQ_D2ZFgi3_xcXZb7O2l_nmEE2WuYV4SototjmexNyav-Xmlc3ZyLX4GPZtT5re2ovAztcQ2g3OLgPr_WWRIi53rXSXJxngiKN7xHzGDpZjQpcAqeMEuySh3tgXJ5ZMbo5sNU8qjSJfv9TbmP2iJi8nE5G_pKZ-AkxTOsV_Vh0Z6BWwL2G15Vlq0KelaAkYrGVCzZJSDLBjO7kQUCuLbtZwuUzNnYkBfp1iVsdO3cyvxUiz33ab9OvmpT8SUHLoeXbO9lDDf2sQGMExf0gIvB1ohIL-PkTCumrIsUsYcKksvSItCx4r2Qgz2-nHvjZbnf3q6FURqkZiI2J_9AKmn76PCBu2BW8Fc0X4Fr7PCmArbaN8FQqmExChAI8YZNRVtbrcRVOKkcDRZE7ALJ3NZYCeYfymfwbErpKhpOo2wxDyvvMeUCWqubHsNj8JHzujoz1qIgN1bw&token_type=Bearer&expires_in=3600&id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI9NiJ9.eyJub25jZSI6ImExNTZhOWRiMDIyMmMzN2U4Y2VkN2FjNmFjNzFmN2ZlNzEyMmMwMDU0MThkYzM0Y2U3N2E3ODljMGRmYWMxYWQiLCJ0b2tlbl90eXBlIjoiaWiifdG9rZW4iLCJpc3MiOiJodHRwczovL3d3dy5hdXRoaW4uaXIiLCJzdWIiOiJhZDVhNTQxZS1iZDA1LTQ2NGMtYjc3MS01ZGUyYzJkZWUzZWUiLCJhdWQiOiIwYTBmNGJmMy1iZDNmLTQxNjktODM2NC02YTJiMzdlMDEzNzIiLCJleHAiOjE1NDg3OTI4ODcsImlhdCI6MTU0ODc4OTI4NywidHlwIjoiSldUIiwiYXRfaGFzaCI6IlJzeFJ5c0l4bdotc3dIVHlpS0h2YmcifQ.RbVbjuv925MofAXa0jnG905PKIZVx5cMydTdZWLjUethbl9dcG_EpqKfp2wnrr-w0FnO73vtEVjySlFdz0Mq8OTZEocuTbroAvV_4lV9sV5KWgh8RjN5ELn0yQIP3YQC21fTmjZatO2CYylHMFpz7mJadCGMIsFZzUSpHQPhRkXHruOgzAz285GRYD8rrCsxLeIOcNq9fnE4OdQz16KsfOu3QFFLACb1XGf2QrW10M-ZwJn-wvDr3knOheAkWUHOpmDOk4gXyKxI3YLkfYSwUsEJAcvxh3F23KK1Eru26UDDHeV6ZBDb7catFstI5iDbDikh5DFctUZcJCjIjQnGxeEzLrS8WlP7G2-A_WM7qH-aNi7QCDAQFmMQ4RI0rTd2xO_4Z8X-SeH-3vnTL2LHvpW9SQjmat37qQiJpv9G-ydbLaVSdNIx7HDVDvcIinZ_uh-naZrvhzRfSUlvkf74BsbReBBupw2GG-sNvsqS0QsKbCwIfVDSAWjn9a122KgO4XcAvdQ6nhbiARHRrr-J3NDYlQ5uxZhS9jAJ24V-hfzkr0uhCauIuQS-EtsGgePZEVqT4aJ68-ERnRQFDuTl9qpfeQexKWKVox06suCCL0DfniRlU-7EJpOop6KEolTMIppiQi9mT7xXLnbl7-7TdWMskTXVFDyTYuAvD_yFlcc&state=SOMETHING_YOU_WANT
```
### what is in response?
- access_token
- id_token
- scope
- token_type
- expires_in
- state

#### access_token 
Your access_token

#### id_token 
Your id_token

#### scope 
List of space seperated user premissions. this premissions are string and defined upon registration of your app.

#### token_type 
Type of the issued tokens. Mainly `Bearer`

#### state 
Whatever state you had provided.


