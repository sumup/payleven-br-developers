.. _authentication:

Authentication
==============

We have two authentication methods: Access/Secret and User/Password.

Basically all endpoints related to merchant or reports uses ``Access/Secret``  and all endpoints related to process transactions uses ``User/Password``.

Access/Secret
-------------

The Access/Secret method uses private and public keys to generate a unique request.

Get your Access and Secret keys from Payleven. Access keys are like public keys and secret are like private keys. Protect your secret key with your life!

1. Generate a ``body string``, based on body "entity" content.
For that, concatenate all data to be sent into a string in the format below, this string will be part of request token:
| key:value[,key:value...]

2. Run an HMAC with SHA512 on the resulting body string. The HMAC password is the secret key you got from Payleven:

3. Run a SHA1 on the result of the HMAC-SHA512. This will be your signature token.

.. note::

    The body string need to be generated in the same request order.
    
PHP Example:

.. code-block:: php

    $access = 'd9d9a83bf099df0396a8177593c52628f20ad86b';
    $secret = 'b8b5be6b13ff089e02e1102e33399386a1c46700bcee261246d9dfa6ac59d43e95bb90b0ea21b9fb42a64115d61a602d14119c91cf840f36df7194a299a056a2';

    $body = array
    (
        'first_name' => 'FirstName',
        'last_name' => 'LastName',
        'gender' => 'M',
        'phone' => '(11) 1234-1234',
        'tax_id' => '96422920415',
        'email' => 'random+7434@domain.com',
        'merchant_reference' => 'GKQ2JD5BSW5IOZZZCSBB',
        'timestamp' => '2015-08-26 10:20:05 -0300'
    );


	Body String Result:

.. code-block:: php

    $bodyString ="first_name:FirstName,last_name:LastName,gender:M,telephone:(11) 1234-1234,cpf:96422920415,email:random+7434@domain.com,merchant_reference:GKQ2JD5BSW5IOZZZCSBB,timestamp:2015-08-26 10:20:05 -0300";

    $sha512 = hash_hmac('sha512', $bodyString, $secretKey);


    HMAC 512 Result:

.. code-block:: php

    $sha512 = 'ef04002d738e4242438343706b191cfaddd3c347b2250e8cc8bd979a6ac4233c0c4aa07243582cc314ee9d9be27d68e0a50d60ea639b8d8258832f4e50184077'

    $requestSignatureToken = sha1($sha512);


	SHA1 Signature Result:

.. code-block:: php

    $requestSignatureToekn = '2bc1f1d4efc5a28cf71d3d6d7ff4631ed6bf5fcb';

The final request needs to be as below:

.. code-block:: php

    $request = array(
        'entity' => array(
            'first_name' => 'FirstName',
            'last_name' => 'LastName',
            'gender' => 'M',
            'phone' => '(11) 1234-1234',
            'tax_id' => '96422920415',
            'email' => 'random+7434@domain.com',
            'merchant_reference' => 'GKQ2JD5BSW5IOZZZCSBB',
            'timestamp' => '2015-08-26 10:20:05 -0300'
        ),
        'access' => 'd9d9a83bf099df0396a8177593c52628f20ad86b',
        'token'  => '2bc1f1d4efc5a28cf71d3d6d7ff4631ed6bf5fcb'
    );


User/Password
-------------

Get your user and password from payleven.

Send a POST request to the authenticate method described above.

.. code-block:: php

    username: payleven-test
    password: e1c8d6347c0c24e5cbc60e508f3fc1b5

Grab the access token from the response.

.. code-block:: php

    {
        "access_token": "ABCDEF123456"
    }

Use it in the subsequent request headers like this (supposing an access token "ABCDEF123456"):

.. code-block:: php

    POST /partner/create-token HTTP/1.1
    Host: staging-api.payleven.com.br
    X-PARTNER-TOKEN: ABCDEF123456
    Cache-Control: no-cache
    Content-Type: multipart/form-data;
    ...