---
layout: post
title: ExactTarget SOAP Upsert with PHP
comments: true
categories: [dev, php, exacttarget, soap]
tags: 
- PHP
status: publish
type: post
published: true
meta: 
  _edit_lock: "1332936793"
  _edit_last: "1"
  _syntaxhighlighter_encoded: "1"
  _aioseop_title: ExactTarget PHP SOAP API upsert
  _aioseop_description: How to perform an UPSERT against the ExactTarget SOAP API using PHP.
  _aioseop_keywords: exacttarget,soap,php,api,upsert
  _wp_old_slug: ""
---
There is quite a bit of documentation for the ExactTarget SOAP API that includes lots of code samples. But unfortunately the PHP code samples are not quite as plentiful as the .NET and Java ones.
After lots of searching and lot of trial and error I finally got my upsert working. An upsert is a SOAP method in the ExactTarget SOAP API that will either update or insert a record depending on whether it is already present or not.
<!--more-->
``` php
<?php
$uo = new ExactTarget_UpdateOptions();
$uo->SaveOptions = array();

$so = new ExactTarget_SaveOption();
$so->PropertyName = 'DataExtensionObject';
$so->SaveAction = ExactTarget_SaveAction::UpdateAdd;

$uo->SaveOptions[] = $so;
$uoSo = ETCore::toSoapVar($uo, 'UpdateOptions');

$request = new ExactTarget_UpdateRequest();
$request->Options = $uoSo;
$request->Objects = array($deoSo);
$result = $soapClient->Update($request);
```

Where *$deoSo* is a DataExtension Object that has been converted to a SoapVar and *$soapClient* is an ExactTarget soap client instance. The way you get the upsert behavior is to set the saveaction to *ExactTarget_SaveAction::UpdateAdd*.

If you are looking for a minimal wrapper library that includes the above method take a look at <a href="https://github.com/hglattergotz/ExactTarget-PHP-SOAP-API">this library on gitub</a>. This library gleans quite a few things from the Doctrine ORM, so if you are familiar with that you should feel right at home.
