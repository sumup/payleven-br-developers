.. _intro:

Introduction
============

Payleven's API provides a easy way to allow you register sellers or service providers - merchants. After being integrated, all registered merchants can use our platform to process transactions and receive payments.

What should I know about payleven integration?
----------------------------------------------

* All merchants need to provide required fields (see table ``Fields Description`` for all examples)
* The marketplace need to provide a endpoint to receive all merchant's status transitions
* Only **ACTIVATED** merchants (approved on onboarding process) can use payleven to process transactions
* **REFUSED** merchants can't use payleven to process transactions

Requirements
------------

* Previous registration at payleven Connect
* **Access Key** (public) , **Secret Key** (private), **Username** and  **Password**

Workflow
--------

Basically, the process starts on merchant registration. Payleven Onboarding Team will analyze merchant information and accept or refuse them. If they were accepted, the merchant will be activated and now be available to process payments.

The next step is create a card token, after that the transaction process can be executed in 2 ways:

1. Authorize and Capture in the same request
2. Authorize and wait to capture until 2 days after

.. note::

    All authorized transactions not captured will expire after 2 days of the authorization date


Partner needs to know when the merchant will be able  to receive payments. The partner needs to provide a endpoint to payleven updating the merchant `STATUS`.

Partner Update status endpoint
------------------------------

The endpoint will receive a POST with the following parameters:

.. code-block:: php

	'merchant_reference'
	'payleven_id'
	'status'

**Fields Description**

+----------+--------------------+--------------+------+----------------------------------------------------+
| Required | Field              | Type         | Size | Description                                        |
+==========+====================+==============+======+====================================================+
| YES      | merchant_reference | Alphanumeric | 128  | UniqueID generated in partner registration process |
+----------+--------------------+--------------+------+----------------------------------------------------+
| YES      | payleven_id        | Numeric      | 11   | payleven UniqueID                                  |
+----------+--------------------+--------------+------+----------------------------------------------------+
| YES      | status             | Alphanumeric | 20   |                                                    |
+----------+--------------------+--------------+------+----------------------------------------------------+

.. warning::

    The URL needs to be sent to payleven team to register the callback on our system
