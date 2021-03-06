GAuthify-PHP
===============
This is the php API Client for [GAuthify](http://www.gauthify.com). The GAuthify REST api helps websites quickly add multi-factor authentication through Google Authenticator, SMS, and Email. This package is a simple wrapper around that api.


Installation
--------------
This library requires and automatically installs requests.

To install simply add gauthify.php into your project directory.

To run a quick test to make sure everything works fine run:

    require_once("gauthify.php");
    $gauthify = new GAuthify(<api_key>)
    $gauthify->quick_test(<test_email>(optional), <test_number>(optional))

Usage
--------------
####Initiate:####
First get an API key by signing up for an account [here](http://www.gauthify.com).

First instantiate a GAuthify object:

    require_once("gauthify.php");
    $auth_instance = new GAuthify(<api_key>);



####Create User:####

    $auth_instance->create_user(<unique_id>, <display_name>)

* unique_id: An id to identify user. Could be the PK used for the user in your db.
* display_name: Name that will be displayed on the library
* Returns: The user hash or raises Error

The user hash returned will have paramaters outlined on the GAuthify.com dashboard page. You can show the user the QR code to scan in their google authenticator applicatoin or you can link/iframe the instructions url.

####Delete User:####∂

    $auth_instance->delete_user(<unique_id>);

* unique_id: An id to identify user. Could be the PK used for the user in your db.
* Returns: True or raises Error

####Get All Users:####

    $auth_instance->get_all_users();

* Returns: List of user hashes

####Get User:####

    $auth_instance->get_user(<unique_id>)

* unique_id: An id to identify user. Could be the PK used for the user in your db.
* Returns: User hash or raises Error

####Check Auth Code:####

    $auth_instance->check_auth(<unique_id>, <auth_code>, safe_mode = False)

* unique_id: An id to identify user. Could be the PK used for the user in your db.
* auth_code: Code retrieved from Google Authenticator, SMS, EMail, or OTP
* safe_mode: If set to true, all exceptions during the request will be suppressed and the check will return True. This essentially temporary bypasses 2-factor authentication if there is a unusualy server error.
* Return: True/False (bool) or raises Error


####Send SMS:####

    $auth_instance->send_sms(<unique_id>, <phone_number>)

* unique_id: An id to identify user. Could be the PK used for the user in your db.
* phone_number: A valid phone number (Currently US only!)
* Returns: User hash or raises Error

####Send Email:####

    $auth_instance->send_email(<email>, <phone_number>)

* unique_id: An id to identify user. Could be the PK used for the user in your db.
* email: A valid email
* Returns: User hash or raises Error

Errors
--------------
The primary error class is GAuthifyError, it can be used as follows:

    require('gauthify.php')

    try{
        <code here>
    }
    catch (GAuthifyError $e){
        print(e->msg) # The error message
        print(e->http_status) # The http status code
        print(e->error_code) # A error code listed in the GAuthfiy application
        print(e->response_body) # The raw http response
    }

The following errors extend GAuthifyError:

* ApiKeyError - Wraps 401 responses (api key issues)
* RateLimitError - Wraps 402 responses (Plan limits, etc)
* ParameterError - Wraps 406 response (Bad formatted unique_id, sms, phone number ,etc)
* NotFoundError - Wraps 404 error (requesting a unique_id that doesn't exist)
* ServerError - Wraps 500 and other server errors
