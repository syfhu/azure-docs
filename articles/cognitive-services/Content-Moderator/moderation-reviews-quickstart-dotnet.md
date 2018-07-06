---
title: Azure Content Moderator - Create reviews using .NET | Microsoft Docs
description: How to create reviews using Azure Content Moderator SDK for .NET
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/04/2018
ms.author: sajagtap
---

# Create reviews using .NET

This article provides information and code samples to help you get started using 
the Content Moderator SDK for .NET to:
 
- Create a set of reviews for human moderators
- Get the status of existing reviews for human moderators

Generally, content goes through some automated moderation before 
being scheduled for human review. This article only covers how to create
the review for human moderation. For a more complete scenario, see the 
[Facebook content moderation](facebook-post-moderation.md) and
[eCommerce catalog moderation](ecommerce-retail-catalog-moderation.md)
tutorials.

This article assumes that you are already familiar with Visual Studio and C#.

## Sign up for Content Moderator services

Before you can use Content Moderator services through the REST API or the SDK, you need a subscription key.
Refer to the [Quickstart](quick-start.md) to learn how you can obtain the key.

## Create your Visual Studio project

1. Add a new **Console app (.NET Framework)** project to your solution.

   In the sample code, name the project **CreateReviews**.

1. Select this project as the single startup project for the solution.

1. Add a reference to the **ModeratorHelper** project assembly that you created
   in the [Content Moderator client helper quickstart](content-moderator-helper-quickstart-dotnet.md).

### Install required packages

Install the following NuGet packages:

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Newtonsoft.Json

### Update the program's using statements

Modify the program's using statements.

	using Microsoft.CognitiveServices.ContentModerator;
	using Microsoft.CognitiveServices.ContentModerator.Models;
	using ModeratorHelper;
	using Newtonsoft.Json;
	using System;
	using System.Collections.Generic;
	using System.IO;
	using System.Threading;

## Create a class to associate internal content information with a review ID

Add the following class to the **Program** class.
Use this class to associate the review ID to 
your internal content ID for the item.

	/// <summary>
	/// Associates the review ID (assigned by the service) to the internal
	/// content ID of the item.
	/// </summary>
	public class ReviewItem
	{
    	/// <summary>
    	/// The media type for the item to review.
    	/// </summary>
    	public string Type;

    	/// <summary>
    	/// The URL of the item to review.
    	/// </summary>
    	public string Url;

    	/// <summary>
    	/// The internal content ID for the item to review.
    	/// </summary>
    	public string ContentId;

    	/// <summary>
    	/// The ID that the service assigned to the review.
    	/// </summary>
    	public string ReviewId;
	}

### Initialize application-specific settings

> [!NOTE]
> Your Content Moderator service key has a requests per second (RPS)
> rate limit, and if you exceed the limit, the SDK throws an exception with a 429 error code. 
>
> A free tier key has a one RPS rate limit.

#### Add the following constants to the **Program** class in Program.cs.
    
    /// <summary>
    /// The minimum amount of time, in milliseconds, to wait between calls
    /// to the Image List API.
    /// </summary>
    private const int throttleRate = 3000;

    /// <summary>
    /// The number of seconds to delay after a review has finished before
    /// getting the review results from the server.
    /// </summary>
    private const double latencyDelay = 45;

    /// <summary>
    /// The name of the log file to create.
    /// </summary>
    /// <remarks>Relative paths are ralative the execution directory.</remarks>
    private const string OutputFile = "OutputLog.txt";

#### Add the following constants and static fields to the **Program** class in Program.cs.

Update these values to contain information specific to your subscription and team.

> [!NOTE]
> You set the TeamName constant to the name you used when you
> created your [Content Moderator review tool](https://contentmoderator.cognitive.microsoft.com/) subscription. 
> You retrieve the TeamName from the **Credentials** section in the **Settings** (gear) menu.
>
> Your team name is the value of the **Id** field in the **API** section.

    /// <summary>
    /// The name of the team to assign the review to.
    /// </summary>
    /// <remarks>This must be the team name you used to create your 
    /// Content Moderator account. You can retrieve your team name from
    /// the Conent Moderator web site. Your team name is the Id associated 
    /// with your subscription.</remarks>
    private const string TeamName = "{teamname}";

    /// <summary>
    /// The optional name of the subteam to assign the review to.
    /// </summary>
    private const string Subteam = null;

    /// <summary>
    /// The callback endpoint for completed reviews.
    /// </summary>
    /// <remarks>Revies show up for reviewers on your team. 
    /// As reviewers complete reviews, results are sent to the
    /// callback endpoint using an HTTP POST request.</remarks>
    private const string CallbackEndpoint = "{callbackUrl}";

    /// <summary>
    /// The media type for the item to review.
    /// </summary>
    /// <remarks>Valid values are "image", "text", and "video".</remarks>
    private const string MediaType = "image";

    /// <summary>
    /// The URLs of the images to create review jobs for.
    /// </summary>
    private static readonly string[] ImageUrls = new string[] {
        "https://moderatorsampleimages.blob.core.windows.net/samples/sample1.jpg"
        // add more if you want
    };

    /// <summary>
    /// The metadata key to initially add to each review item.
    /// </summary>
    private const string MetadataKey = "sc";

    /// <summary>
    /// The metadata value to initially add to each review item.
    /// </summary>
    private const string MetadataValue = "true";

#### Add the following static fields to the **Program** class in Program.cs.

Use these fields to track the application state.

    /// <summary>
    /// A static reference to the text writer to use for logging.
    /// </summary>
    private static TextWriter writer;

    /// <summary>
    /// The cached review information, associating a local content ID
    /// to the created review ID for each item.
    /// </summary>
    private static List<ReviewItem> reviewItems =
        new List<ReviewItem>();

## Create a method to write messages to the log file

Add the following method to the **Program** class. 

	/// <summary>
	/// Writes a message to the log file, and optionally to the console.
	/// </summary>
	/// <param name="message">The message.</param>
	/// <param name="echo">if set to <c>true</c>, write the message to the console.</param>
	private static void WriteLine(string message = null, bool echo = false)
	{
    	writer.WriteLine(message ?? String.Empty);

    	if (echo)
    	{
    	    Console.WriteLine(message ?? String.Empty);
    	}
	}

## Create a method to create a set of reviews

Normally, you have some business logic for identifying incoming images, text,
or video that needs to be reviewed. However, here just use a fixed list
of images.

Add the following method to the **Program** class.

	/// <summary>
	/// Create the reviews using the fixed list of images.
	/// </summary>
	/// <param name="client">The Content Moderator client.</param>
	private static void CreateReviews(ContentModeratorClient client)
	{
    	WriteLine(null, true);
    	WriteLine("Creating reviews for the following images:", true);

    	// Create the structure to hold the request body information.
    	List<CreateReviewBodyItem> requestInfo =
        	new List<CreateReviewBodyItem>();

    	// Create some standard metadata to add to each item.
    	List<CreateReviewBodyItemMetadataItem> metadata =
        	new List<CreateReviewBodyItemMetadataItem>(
            new CreateReviewBodyItemMetadataItem[] {
                new CreateReviewBodyItemMetadataItem(
                    MetadataKey, MetadataValue)
            });

    	// Populate the request body information and the initial cached review information.
    	for (int i = 0; i < ImageUrls.Length; i++)
    	{
        	// Cache the local information with which to create the review.
        	var itemInfo = new ReviewItem()
        	{
            	Type = MediaType,
            	ContentId = i.ToString(),
            	Url = ImageUrls[i],
            	ReviewId = null
        	};

        	WriteLine($" - {itemInfo.Url}; with id = {itemInfo.ContentId}.", true);

        	// Add the item informaton to the request information.
        	requestInfo.Add(new CreateReviewBodyItem(
            	itemInfo.Type, itemInfo.Url, itemInfo.ContentId,
            	CallbackEndpoint, metadata));

        	// Cache the review creation information.
        	reviewItems.Add(itemInfo);
    	}

    	var reviewResponse = client.Reviews.CreateReviewsWithHttpMessagesAsync(
        	"application/json", TeamName, requestInfo);

    	// Update the local cache to associate the created review IDs with
    	// the associated content.
    	var reviewIds = reviewResponse.Result.Body;
    	for (int i = 0; i < reviewIds.Count; i++)
    	{
        	Program.reviewItems[i].ReviewId = reviewIds[i];
    	}

    	WriteLine(JsonConvert.SerializeObject(
        reviewIds, Formatting.Indented));

    	Thread.Sleep(throttleRate);
	}

## Create a method to get the status of existing reviews

Add the following method to the **Program** class. 

> [!Note]
> In practice, you would set the callback URL `CallbackEndpoint` to the URL
> that would receive the results of the manual review (via an HTTP POST request).
> You could modify this method to check on the status of pending reviews.

	/// <summary>
	/// Gets the review details from the server.
	/// </summary>
	/// <param name="client">The Content Moderator client.</param>
	private static void GetReviewDetails(ContentModeratorClient client)
	{
    	WriteLine(null, true);
    	WriteLine("Getting review details:", true);
    	foreach (var item in reviewItems)
    	{
        	var reviewDetail = client.Reviews.GetReviewWithHttpMessagesAsync(
            	TeamName, item.ReviewId);

        	WriteLine(
            	$"Review {item.ReviewId} for item ID {item.ContentId} is " +
            	$"{reviewDetail.Result.Body.Status}.", true);
        	WriteLine(JsonConvert.SerializeObject(
            	reviewDetail.Result.Body, Formatting.Indented));

        	Thread.Sleep(throttleRate);
    	}
	}

## Add code to create a set of reviews and check its status

Add the following code to the **Main** method.

This code simulates many of the operations that you perform in defining and
managing the list, as well as using the list to screen images. The logging features
allow you to see the response objects generated by the SDK calls to the Content
Moderator service.

	using (TextWriter outputWriter = new StreamWriter(OutputFile, false))
	{
    	writer = outputWriter;
    	using (var client = Clients.NewClient())
    	{
        	CreateReviews(client);
        	GetReviewDetails(client);

        	Console.WriteLine();
        	Console.WriteLine("Perform manual reviews on the Content Moderator site.");
        	Console.WriteLine("Then, press any key to continue.");
        	Console.ReadKey();

        	Console.WriteLine();
        	Console.WriteLine($"Waiting {latencyDelay} seconds for results to propigate.");
        	Thread.Sleep(latencyDelay * 1000);

        	GetReviewDetails(client);
    	}

    	writer = null;
    	outputWriter.Flush();
    	outputWriter.Close();
	}

	Console.WriteLine();
	Console.WriteLine("Press any key to exit...");
	Console.ReadKey();

## Run the program and review the output

You see the following sample output:

	Creating reviews for the following images:
		- https://moderatorsampleimages.blob.core.windows.net/samples/sample1.jpg; with id = 0.

	Getting review details:
	Review 201712i46950138c61a4740b118a43cac33f434 for item ID 0 is Pending.

Sign into the Content Moderator review tool to see the pending image review with the **sc** label set to **true**. You also see the default **a** and **r** tags and any custom tags that you may have defined within the review tool. 

Use the **Next** button to submit.

![Image review for human moderators](images/moderation-reviews-quickstart-dotnet.PNG)

Then, press any key to continue.

	Waiting 45 seconds for results to propagate.

	Getting review details:
	Review 201712i46950138c61a4740b118a43cac33f434 for item ID 0 is Complete.

	Press any key to exit...

## Check out the following output in the log file.

> [!NOTE]
> In your output file, the strings "\{teamname}" and "\{callbackUrl}" reflect the 
> values for the `TeamName` and `CallbackEndpoint` fields, respectively.

The review IDs and the image content URLs are different each time you run 
the application, and when a review is complete, the `reviewerResultTags` field 
reflects how the reviewer tagged the item.

	Creating reviews for the following images:
		- https://moderatorsampleimages.blob.core.windows.net/samples/sample1.jpg; with id = 0.
	[
		"201712i46950138c61a4740b118a43cac33f434",
	]

	Getting review details:
	Review 201712i46950138c61a4740b118a43cac33f434 for item ID 0 is Pending.
	{
		"reviewId": "201712i46950138c61a4740b118a43cac33f434",
		"subTeam": "public",
		"status": "Pending",
		"reviewerResultTags": [],
		"createdBy": "{teamname}",
		"metadata": [
    	{
      		"key": "sc",
      		"value": "true"
    	}
		],
		"type": "Image",
		"content": "https://reviewcontentprod.blob.core.windows.net/{teamname}/IMG_201712i46950138c61a4740b118a43cac33f434",
		"contentId": "0",
		"callbackEndpoint": "{callbackUrl}"
	}

	Getting review details:
	Review 201712i46950138c61a4740b118a43cac33f434 for item ID 0 is Complete.
	{
		"reviewId": "201712i46950138c61a4740b118a43cac33f434",
		"subTeam": "public",
		"status": "Complete",
		"reviewerResultTags": [
    	{
      		"key": "a",
      		"value": "False"
    	},
    	{
      		"key": "r",
      		"value": "True"
    	},
    	{
      		"key": "sc",
      		"value": "True"
    	}
		],
		"createdBy": "{teamname}",
		"metadata": [
    	{
      		"key": "sc",
      		"value": "true"
    	}
		],
		"type": "Image",
		"content": "https://reviewcontentprod.blob.core.windows.net/{teamname}/IMG_201712i46950138c61a4740b118a43cac33f434",
		"contentId": "0",
		"callbackEndpoint": "{callbackUrl}"
	}

## Your callback Url if provided, receives this response

You see a response like the following example:

	{
		"ReviewId": "201801i48a2937e679a41c7966e838c92f5e649",
		"ModifiedOn": "2018-01-06T05:04:40.5525865Z",
		"ModifiedBy": "yourusername",
		"CallBackType": "Review",
		"ContentId": "0",
		"ContentType": "Image",
		"Metadata": {
    		"sc": "true"
			},
		"ReviewerResultTags": {
			"a": "False",
			"r": "False",
		}
	}


## Next steps

[Download the Visual Studio solution](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) for this and other Content Moderator quickstarts for .NET, and get started on your integration.
