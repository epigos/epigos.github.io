---
layout: post
title: Image Manipulation with PHP – The GD Libraries
description: Image Manipulation with PHP using The GD Libraries to resize, reduce, rotate,create thumbnails from an image
keywords: Image ,Manipulation , PHP , GD Libraries, resize, reduce, rotate,create thumbnails, from an image
author: Philip Adzanoukpe
date: 2013-04-27 12:33:28
updated: 2013-04-27 12:33:28
tags: [php, image]
categories: post
---

The GD libraries are the principle PHP module used for image manipulation. If you are lucky enough to be hosted on (or indeed own) a server running GD2.0 or above, you’ll have the ability to use truecolour images in jpeg format (and in png if version 2.0.4+) and therefore, you won’t really benefit by reading this tutorial.

Those using GD2.0+ should be opting to use ImageCreateTrueColor in place of ImageCreate, and ImageCopyResampled in place of ImageCopyResized to assure use of maximum colour levels. If you’re running anything under 2.0, read on! This article will do the following 1. thumbnail 2. resize (actual image) 3. reduce (image file size) 4. rotate (by degrees)

Create a thumbnail

{% highlight php %}
<?php
function createThumbnail($img, $imgPath, $suffix, $newWidth, $newHeight, $quality) {
    // Open the original image.
    $original = imagecreatefromjpeg("$imgPath/$img") or die("Error Opening original");
    list($width, $height, $type, $attr) = getimagesize("$imgPath/$img");
    // Resample the image.
    $tempImg = imagecreatetruecolor($newWidth, $newHeight) or die("Cant create temp image");
    imagecopyresized($tempImg, $original, 0, 0, 0, 0, $newWidth, $newHeight, $width, $height) or die("Cant resize copy");
    // Create the new file name.
    $newNameE = explode(".", $img);
    $newName = ''. $newNameE[0] .''. $suffix .'.'. $newNameE[1] .'';
    // Save the image.
    imagejpeg($tempImg, "$imgPath/$newName", $quality) or die("Cant save image");
    // Clean up.
    imagedestroy($original);
    imagedestroy($tempImg);
    return true;
}
?>
{% endhighlight %}

Rotate

{% highlight php %}
<?php
function rotateImage($img, $imgPath, $suffix, $degrees, $quality, $save) {
    // Open the original image.
    $original = imagecreatefromjpeg("$imgPath/$img") or die("Error Opening original");
    list($width, $height, $type, $attr) = getimagesize("$imgPath/$img");
    // Resample the image.
    $tempImg = imagecreatetruecolor($width, $height) or die("Cant create temp image");
    imagecopyresized($tempImg, $original, 0, 0, 0, 0, $width, $height, $width, $height) or die("Cant resize copy");
    // Rotate the image.
    $rotate = imagerotate($original, $degrees, 0);
    // Save.
    if($save)
    {
        // Create the new file name.
    $newNameE = explode(".", $img);
    $newName = ''. $newNameE[0] .''. $suffix .'.'. $newNameE[1] .'';
    // Save the image.
    imagejpeg($rotate, "$imgPath/$newName", $quality) or die("Cant save image");
    }
    // Clean up.
    imagedestroy($original);
    imagedestroy($tempImg);
    return true;
}
?>
{% endhighlight %}

Resize

{% highlight php %}
<?php
function resizeImage($img, $imgPath, $suffix, $by, $quality){
    // Open the original image.
    $original = imagecreatefromjpeg("$imgPath/$img") or die("Error Opening original (<em>$imgPath/$img</em>)");
    list($width, $height, $type, $attr) = getimagesize("$imgPath/$img");
    // Determine new width and height.
    $newWidth = ($width/$by);
    $newHeight = ($height/$by);
    // Resample the image.
    $tempImg = imagecreatetruecolor($newWidth, $newHeight) or die("Cant create temp image");
    imagecopyresized($tempImg, $original, 0, 0, 0, 0, $newWidth, $newHeight, $width, $height) or die("Cant resize copy");
    // Create the new file name.
    $newNameE = explode(".", $img);
    $newName = ''. $newNameE[0] .''. $suffix .'.'. $newNameE[1] .'';
    // Save the image.
    imagejpeg($tempImg, "$imgPath/$newName", $quality) or die("Cant save image");
    // Clean up.
    imagedestroy($original);
    imagedestroy($tempImg);
    return true;
}
?>
{% endhighlight %}

Reduce

{% highlight php %}
<?php
function reduceImage ($img, $imgPath, $suffix, $quality) {
    // Open the original image.
    $original = imagecreatefromjpeg("$imgPath/$img") or die("Error Opening original (<em>$imgPath/$img</em>)");
    list($width, $height, $type, $attr) = getimagesize("$imgPath/$img");
    // Resample the image.
    $tempImg = imagecreatetruecolor($width, $height) or die("Cant create temp image");
    imagecopyresized($tempImg, $original, 0, 0, 0, 0, $width, $height, $width, $height) or die("Cant resize copy");
    // Create the new file name.
    $newNameE = explode(".", $img);
    $newName = ''. $newNameE[0] .''. $suffix .'.'. $newNameE[1] .'';
    // Save the image.
    imagejpeg($tempImg, "$imgPath/$newName", $quality) or die("Cant save image");
    // Clean up.
    imagedestroy($original);
    imagedestroy($tempImg);
    return true;
}
?>
{% endhighlight %}

The simple instructions I used for the examples

{% highlight php %}
<?php
$thumb = createThumbnail($img, $imgPath, "-thumb", 120, 100, 100);
$rotate = rotateImage($img, $imgPath, "-rotated", 270, 100, 1);
$resize = resizeImage($img, $imgPath, "-resized", 2, 100);
$reduce = reduceImage($img, $imgPath, "-reduced", 70);
?>
{% endhighlight %}
