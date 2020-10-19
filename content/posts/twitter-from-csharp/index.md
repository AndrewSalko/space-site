---
title: "Posting tweet from C#"
date: 2020-10-17T10:35:35-05:00
description: "How to post twitter message with image from .NET Core."
categories: [".NET Core"]
dropCap: true
displayInMenu: false
displayInList: true
draft: false
resources:
- name: featuredImage
  src: "mdd-iphone.jpg"
---

To tweet programmatically, you need one of the libraries. In this example, I will consider [Tweetinvi](https://github.com/linvi/tweetinvi). You need just add nuget package to your project.

## Get Twitter API key, token and secrets

Go to the [developer.twitter.com](https://developer.twitter.com), and get access to the Twitter API. 
You will got params, required for TwitterClient class.


## Code


```csharp

string tweetText="This is text!";
byte[] imageFileBody=null; //... load image (like File.ReadAllBytes)

var userClient = new TwitterClient(twitterAPIKey, twitterAPISecret, twitterAccessToken, twitterAccessTokenSecret);
var uploadTweetImageParameters = new Tweetinvi.Parameters.UploadTweetImageParameters(imageFileBody);

var uploadedImage = await userClient.Upload.UploadTweetImageAsync(uploadTweetImageParameters);

if (uploadedImage.Id == null)
	throw new ApplicationException("uploadedImage.Id == null");

long mediaID = uploadedImage.Id.Value;

var publishTweetParameters = new Tweetinvi.Parameters.PublishTweetParameters(tweetText);
publishTweetParameters.MediaIds.Add(mediaID);

await userClient.Tweets.PublishTweetAsync(publishTweetParameters);
```
If you need to post tweet with text and image, you need upload image first. Secondary, use image mediaID, and add it to the PublishTweetParameters object.
Also, keep in mind that Twitter has limitations on image size (about 5 Mb for images and 15 Mb for .gif and video).

You can see the class for tweets in a more complex project: [TwitterService](https://github.com/AndrewSalko/kawaii.twitter.azure/tree/master/kawaii.twitter.core/TwitterService). 


