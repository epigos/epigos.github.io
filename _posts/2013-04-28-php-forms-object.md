---
layout: post
title: PHP Forms Object
description: A singleton PHP object which provides convenience and fast methods for creating HTML forms with validations. It uses jquery validation plugins to validate forms
keywords: php forms object,forms with php,php html forms,php class for forms
author: Philip Adzanoukpe
date: 2013-04-28 12:33:28
updated: 2013-04-28 12:33:28
tags: [php, forms]
categories: post
---

A singleton PHP object which provides convenience and fast methods for creating HTML forms with validations. It uses jquery validation plugins to validate forms. When you build as many dynamic CMS-like websites as me, you end up having to manipulate single HTML forms attributes with a bunch of logic and validation issues and the code can start looking ugly.

It's small, easy to use, and produces beautiful code and forms The class is pretty simple. When you instantiate the class, you feed it the form attributes. Once the form is created, you use start() to open the form tag and use the createInput(), createTextarea(), createSelectvalues() methods to create your form elements and finally use the end() to close the form tag. You can choose my default jquery validation or use your own validation by setting the $useValidation variable to false.

**Why this Object:**

 1. Ability to create forms easier and faster.
 2. Good creating CMS websites.
 3. Inbuilt jquery validation scripts.

**Basic Example**

{% highlight php %}
<?php

include "htmlforms.php";

//assign form attributes with $useValidation set to true

$form = new HtmlForms(array('name'=>'contact','method'=>'post','id'=>'contact','action'=>'contact.php'),true);

//open form tag

$form->start();

//creates a hidden input.

$form->createInput(null,array(
    'name'=>'Send_contact',
    'id'=>'Send_contact',
    'type'=>'hidden',
    'value'=>'yes'

));

//create an input with validation.

$form->createInput('Enter your Email',array(
     //attributes as array
    'name'=>'email',
    'id'=>'email',
    'type'=>'text',
    'title'=>'Enter your Email'

),array(
    //validation as array

    'required'=>'true',
    'email'=>'true'
) );

//creates a textarea without validation.

$form->createTextarea('Enter your message',array(
    'name'=>'message',
    'id'=>'message',
    'title'=>'Enter your message',

));

// create a select option.

$form->createSelect('Select type',array(
    'name'=>'type',
    'id'=>'type',
    'title'=>'Select type'
    ),array(
        'required'=>'true'
),
array(
   //options as array
    'Select Type...'=>array('value'=>""),
    'Free'=>array('value'=>'free'),
    'Premium'=>array('value'=>'premium')
) );

//close form tag.

$form->end();

?>
{% endhighlight %}

[Code on github][1]


  [1]: http://epigos.github.com/HtmlForms/
