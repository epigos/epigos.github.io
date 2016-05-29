---
layout: post
title: Using Arrays with Form Elements - PHP
description: Using Arrays with Form Elements in PHP to generate form fields and process forms
keywords: array form php,php array,array get php,form array php
author: Philip Adzanoukpe
date: 2013-04-26 12:33:28
updated: 2013-04-26 12:33:28
tags: php, forms
categories: post
---

Arrays are particularly useful when dealing with table-like HTML form data. Suppose we want to allow a user to enter a number of names into a database. The HTML code for the text-boxes could have looked like this:

{% highlight php %}
<INPUT NAME="first1" TYPE=TEXT>

<INPUT NAME="last1" TYPE=TEXT>

<INPUT NAME="first2" TYPE=TEXT>

<INPUT NAME="last2" TYPE=TEXT>

<INPUT NAME="first3" TYPE=TEXT>

<INPUT NAME="last3" TYPE=TEXT>
{% endhighlight %}

Each text-box has a unique name ("first1", "last1", "first2", etc.). When the form is submitted, this will result in a PHP variable for every text-box on the form ($first1, $last1, $first2, etc.). However, if the number of rows can vary, as it likely will, when dealing with data, it is more practical to represent each field as an array of data. In HTML, this is done by placing square brackets at the end of the element name. We can use a PHP for loop to place the requisite number of text boxes on the form:

{% highlight php %}
<?php
$names = 3;
?>
<FORM ACTION="submit.php" METHOD=POST>
<? for ($i = 1; $i <= $names; $i++): ?>
First name:
<INPUT NAME="first[]" TYPE=TEXT><BR>
Last name:
<INPUT NAME="last[]" TYPE=TEXT><BR><BR>
<? endfor ?>
<BR><INPUT TYPE=SUBMIT>
</FORM>
{% endhighlight %}

When the form is submitted, PHP will create an arrays called $first and $last, with each element containing the value of a text-box. This gives us the ability to easily process the submitted data in our PHP script. Suppose that we wish to add the submitted names to a database table. We can easily build a SQL INSERT statement for each pair of names simply by looping through the array:

{% highlight php %}
<?php
$numrows = count ($first);
for ($i = 0; $i < $numrows; ++$i) {
$sql = "INSERT INTO Names ('First', 'Last') " .
"VALUES ('$first[$i]', '$last[$i]')";
// Code to execute query goes here.
// ...
// ...
}
?>
{% endhighlight %}
