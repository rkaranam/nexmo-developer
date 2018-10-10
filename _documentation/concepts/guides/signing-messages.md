---
title: Signing requests
description: Add an extra layer of security by sending and receiving signed requests.
navigation_weight: 4
---

# Signing requests

Signatures validate the authenticity of the person who interacts with Nexmo.

You use a signature to:

* Verify that a request originates from a trusted source
* Ensure that the message has not been tampered with en-route
* Defend against interception and later replay

We support a number of different signing algorithms. You can choose one from the dashboard. The default is 'MD5 Hash'.

You should use your Nexmo library's ability to generate or validate signatures. If you _can't_ do this for some reason, you _can_ generate and validate signatures yourself, but this can be complicated and potentially error-prone.

> Note: Using signatures is an *optional* improvement on using the standard `api_secret`. You use the `SIGNATURE_SECRET` instead of your api_secret in a signed request.

## Generating Signatures

It is _highly recommended_ that you use your Nexmo library's existing functionality for generating and validating signatures. If you aren't using a library with this functionality, you'll need to generate the signature yourself. The technique is slightly different if are generating an 'MD5 hash' signature or one of the HMAC signatures.

## Generating an MD5 Hash Signature

The process for generating and using an MD5 hash signature is as follows:

* Add the current timestamp to the parameters list with the key `timestamp`. This should be an integer containing the number of seconds since the epoch (this is sometimes also known as [UNIX time](https://en.wikipedia.org/wiki/Unix_time))
* Loop through each of the parameters, sorted by key
* For every value in the parameter list, replace all instances of `&` and `=` with an underscore `_`.
* Generate a string consisting of `&akey=value&bkey=value`. **Note that there is an ampersand `&` at the start of the string!**
* Add an ampersand `&` and the signature secret to the end of the string. It should now look something like this: `&akey=value&bkey=value&your_signature_secret`
* Now run the string through an md5 hash function and convert the resulting bytes to a string of hexadecimal digits. This is your MD5 hash signature, and should be added to the HTTP parameters of your request as the `sig` parameter.

Note that although you changed the parameter values while generating the signature, the values passed as HTTP parameters should be __unchanged__.

## Generating an HMAC Signature

Nexmo supports MD5, SHA1, SHA256 & SHA512 HMAC signatures. To generate these signatures you will need an HMAC library that can support one or more of these algorithms.

* Add the current timestamp to the parameters list with the key `time`. This should be an integer containing the number of seconds since the epoch (this is sometimes also known as [UNIX time](https://en.wikipedia.org/wiki/Unix_time))
* Loop through each of the parameters, sorted by key
* For every value in the parameter list, replace all instances of `&` and `=` with an underscore `_`.
* Generate a string consisting of `&akey=value&bkey=value`. **Note that there is an ampersand `&` at the start of the string!**
* Create an HMAC generator with your desired algorithm and your signature secret as the key.
* Now run the string through an hmac generator and convert the resulting bytes to a string of hexadecimal digits. This is your HMAC signature, and should be added to the HTTP parameters of your request as the `sig` parameter.

Note that although you changed the parameter values while generating the signature, the values passed as HTTP parameters should be __unchanged__.

## Validating a Signature

Validating a Nexmo Signature requires generating a signature from an incoming request's parameters and comparing it to the signature that was provided in the request. **Note**: Remember to remove the `sig` parameter before generating your signature, and instead of adding a `timestamp`, use the `timestamp` provided in the request parameters.

The following example shows a signed request to the SMS API:

```
https://rest.nexmo.com/sms/xml?api_key=API_KEY&from=Nexmo&to=447700900000&type=text&text=Hello+from+Nexmo&status-report-req=false&timestamp=1461605396&sig=SIGNATURE
```

The workflow for using signed messages is:

![Signing requests workflow](/assets/images/workflow_call_api_outbound.svg)

1. Create a signed [request](/api/sms#request) to send an SMS.
2. Check the [response codes](/api/sms#errors) and ensure that you sent the request correctly.
3. Your message is delivered to the handset. The user's handset returns a delivery receipt.
4. If you requested signed delivery receipts and inbound messages validate the signature for each incoming request.


## Implementing the message signing workflow

When you create a Nexmo account you will be provided a signature secret. These can be found in your [account settings](https://dashboard.nexmo.com/settings) in the Nexmo Dashboard.

To sign your messages:

1. Create a signed [request](/api/sms#request):

    ```tabbed_examples
    source: '_examples/messaging/signing-messages/create-request'
    ```

2. Check the [response codes](/api/sms#response-codes) to ensure that you sent the request to Nexmo correctly:

    ```tabbed_examples
    source: '_examples/messaging/signing-messages/check-response'
    ```

    If you did not generate the signature correctly the [status](/api/sms#status-codes) is `14, invalid signature`

3. Your message is delivered to the handset. The user's handset returns a delivery receipt.

4. If your delivery receipts and inbound messages are signed, validate the signature:

    ```tabbed_examples
    source: '_examples/messaging/signing-messages/validate-signature'
    ```

## Troubleshooting signatures

Here are some tips and pitfalls to look out for when working with signed messages.

### Check the response for details

If the message isn't sent as expected, check the response for any [error codes](/api/sms#errors) that were returned. This will usually give you more detail on what to do next.

### Error 14: Invalid Signature

If the text being sent includes any special characters such as `&` (ampersand) or `=` (equals), then these need to be replaced in the text used to create the signature.

A general approach would be:

- Detect that the text includes `&` or `=`.
- Create a version of the text that uses `_` (underscore) in place of these special characters.
- Use the sanitised version of the text to create the signature.

The original text can be still be sent/received, the character replacements are only needed to generate the signature.
