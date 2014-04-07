---
layout: post
title: "ExactTarget SOAP API: Obtaining an Accurate List of Retrievable Properties"
date: 2012-06-18 07:00
comments: true
categories: [dev, php, exacttarget, soap]
---
Aahhh! The joys of integrating with third-party API's.
Trying to get your application to talk to a third-party API is sometimes tricky, especially if the API documentation is not up to date.
SOAP is supposed to make this simpler by providing a WSDL from which you can generate classes.
So what are you supposed to do if the documentation and the WSDL seem out of date?
<!-- more -->
**Clarification**: By out of date I mean that the documentation for a particular object states it has retrievable properties *a*, *b* and *c*, the WSDL has properties *a*, *c* and *d*, and by trial and error you finally figure out that only *a* and *d* are retrievable.

Not to fear, ExactTarget provides a SOAP method that describes objects. You pass the object name as a parameter and in return you get a list of all properties and their attributes. One of the attributes is **isRetrievable**, which is what I am after.

Great! Right? Well, mostly...

For most objects the method described below returns accurate results from what I can tell. The only object I have encountered so far that has an incorrect result form this method is [EmailSendDefinition](http://docs.code.exacttarget.com/020_Web_Service_Guide/Objects/EmailSendDefinition).

{% codeblock EmailSendDefinition retrievable properties lang:php %}
Client.ID
CreatedDate
ModifiedDate
ObjectID
CustomerKey
Name
CategoryID
Description
SendClassification.CustomerKey
SenderProfile.CustomerKey
SenderProfile.FromName
SenderProfile.FromAddress
DeliveryProfile.CusomterKey            // <-- Generates error
DeliveryProfile.CustomerKey
DeliveryProfile.SourceAddressType
DeliveryProfile.PrivateIP
DeliveryProfile.DomainType
DeliveryProfile.PrivateDomain
DeliveryProfile.HeaderSalutationSource
DeliveryProfile.HeaderContentArea.ID   // <-- Generates error
DeliveryProfile.FooterSalutationSource
DeliveryProfile.FooterContentArea.ID   // <-- Generates error
SuppressTracking
IsSendLogging
Email.ID
CCEmail
BccEmail
AutoBccEmail
TestEmailAddr
EmailSubject
DynamicEmailSubject
IsMultipart
IsWrapped
SendLimit
SendWindowOpen
SendWindowCloses                       // <-- Generates error
DeduplicateByEmail
ExclusionFilter
Additional
SendDefinitionList
IsPlatformObject
{% endcodeblock %}

The Describe method is quite handy and I use it quite a bit when exploring the API, so I added it to my PHP library for accessing ExactTarget's SOAP API.
You can get the lib [here](https://github.com/hglattergotz/ExactTarget-PHP-SOAP-API), and this is how you would get the definition for the above EmailSendDefinition object:

{% codeblock describe.php lang:php %}
<?php
ETCore::initialize($userName, $password);
print_r(ETCore::getObjectDefinition('EmailSendDefinition'));
{% endcodeblock %}

{% codeblock Result lang:php %}
definition = stdClass Object
(
    [ObjectType] => EmailSendDefinition
    [Properties] => Array
        (
            [0] => stdClass Object
                (
                    [PartnerKey] => 
                    [ObjectID] => 
                    [Name] => Client.ID
                    [DataType] => Int32
                    [IsUpdatable] => 
                    [IsRetrievable] => 1
                    [IsRequired] => 
                )

            [1] => stdClass Object
                (
                    [PartnerKey] => 
                    [ObjectID] => 
                    [Name] => CreatedDate
                    [DataType] => DateTime
                    [IsUpdatable] => 1
                    [IsRetrievable] => 1
                    [IsRequired] => 
                )

            [2] => stdClass Object
                (
                    [PartnerKey] => 
                    [ObjectID] => 
                    [Name] => ModifiedDate
                    [DataType] => DateTime
                    [IsUpdatable] => 1
                    [IsRetrievable] => 1
                    [IsRequired] => 
                )
             :
             :
{% endcodeblock %}
As I learn more about the ExactTarget SOAP API and SOAP itself I will be posting my findings and expanding the library on github.
