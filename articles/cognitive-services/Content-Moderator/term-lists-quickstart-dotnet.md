---
title: Moderate with custom term lists in Azure Content Moderator | Microsoft Docs
description: How to moderate with custom term lists using Azure Content Moderator SDK for .NET.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/11/2018
ms.author: sajagtap
---

# Moderate with custom term lists in .NET

The default global list of terms in Azure Content Moderator is sufficient for most content moderation needs. However, you might need to screen for terms that are specific to your organization. For example, you might want to tag competitor names for further review. 

You can use the Content Moderator SDK for .NET to create custom lists of terms to use with the Text Moderation API.

> [!NOTE]
> There is a maximum limit of **5 term lists** with each list to **not exceed 10,000 terms**.
>

This article provides information and code samples to help you get started using 
the Content Moderator SDK for .NET to:
- Create a list.
- Add terms to a list.
- Screen terms against the terms in a list.
- Delete terms from a list.
- Delete a list.
- Edit list information.
- Refresh the index so that changes to the list are included in a new scan.

This article assumes that you are already familiar with Visual Studio and C#.

## Sign up for Content Moderator services

Before you can use Content Moderator services through the REST API or the SDK, you need a subscription key.

In the Content Moderator Dashboard, you can find your subscription key in **Settings** > **Credentials** > **API** > **Trial Ocp-Apim-Subscription-Key**. For more information, see [Overview](overview.md).

## Create your Visual Studio project

1. Add a new **Console app (.NET Framework)** project to your solution.

1. Name the project **TermLists**. Select this project as the single startup project for the solution.

1. Add a reference to the **ModeratorHelper** project assembly that you created 
   in the [Content Moderator client helper quickstart](content-moderator-helper-quickstart-dotnet.md).

### Install required packages

Install the following NuGet packages for the TermLists project:

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Microsoft.Rest.ClientRuntime.Azure
- Newtonsoft.Json

### Update the program's using statements

Modify the program's using statements.

	using System;
	using System.Threading;
	using Microsoft.CognitiveServices.ContentModerator;
	using Microsoft.CognitiveServices.ContentModerator.Models;
	using ModeratorHelper;

### Add private properties

Add the following private properties to namespace TermLists, class Program.

	/// <summary>
    /// The language of the terms in the term lists.
    /// </summary>
    private const string lang = "eng";

    /// <summary>
    /// The minimum amount of time, in milliseconds, to wait between calls
    /// to the Content Moderator APIs.
    /// </summary>
    private const int throttleRate = 3000;

    /// <summary>
    /// The number of minutes to delay after updating the search index before
    /// performing image match operations against a the list.
    /// </summary>
    private const double latencyDelay = 0.5;

## Create a term list

You create a term list with **ContentModeratorClient.ListManagementTermLists.Create**. The first parameter to **Create** is a string that contains a MIME type, which should be "application/json". For more information, see the [API reference](https://westus2.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f). The second parameter is a **Body** object that contains a name and description for the new term list.

Add the following method definition to namespace TermLists, class Program.

> [!NOTE]
> Your Content Moderator service key has a requests per second (RPS)
> rate limit, and if you exceed the limit, the SDK throws an exception with a 429 error code. 
>
> A free tier key has a one RPS rate limit.

	/// <summary>
    /// Creates a new term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <returns>The term list ID.</returns>
    static string CreateTermList (ContentModeratorClient client)
    {
    	Console.WriteLine("Creating term list.");

        Body body = new Body("Term list name", "Term list description");
        TermList list = client.ListManagementTermLists.Create("application/json", body);
        if (false == list.Id.HasValue)
        {
        	throw new Exception("TermList.Id value missing.");
        }
        else
        {
        	string list_id = list.Id.Value.ToString();
            Console.WriteLine("Term list created. ID: {0}.", list_id);
            Thread.Sleep(throttleRate);
            return list_id;
        }
	}

## Update term list name and description

You update the term list information with **ContentModeratorClient.ListManagementTermLists.Update**. The first parameter to **Update** is the term list ID. The second parameter is a MIME type, which should be "application/json". For more information, see the [API reference](https://westus2.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f685). The third parameter is a **Body** object, which contains the new name and description.

Add the following method definition to namespace TermLists, class Program.

	/// <summary>
    /// Update the information for the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list to update.</param>
    /// <param name="name">The new name for the term list.</param>
    /// <param name="description">The new description for the term list.</param>
    static void UpdateTermList (ContentModeratorClient client, string list_id, string name = null, string description = null)
    {
		Console.WriteLine("Updating information for term list with ID {0}.", list_id);
        Body body = new Body(name, description);
        client.ListManagementTermLists.Update(list_id, "application/json", body);
        Thread.Sleep(throttleRate);
	}

## Add a term to a term list

Add the following method definition to namespace TermLists, class Program.

	/// <summary>
    /// Add a term to the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list to update.</param>
    /// <param name="term">The term to add to the term list.</param>
    static void AddTerm (ContentModeratorClient client, string list_id, string term)
    {
    	Console.WriteLine("Adding term \"{0}\" to term list with ID {1}.", term, list_id);
        client.ListManagementTerm.AddTerm(list_id, term, lang);
        Thread.Sleep(throttleRate);
	}

## Get all terms in a term list

Add the following method definition to namespace TermLists, class Program.

	/// <summary>
    /// Get all terms in the indicated term list.
	/// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list from which to get all terms.</param>
    static void GetAllTerms(ContentModeratorClient client, string list_id)
    {
    	Console.WriteLine("Getting terms in term list with ID {0}.", list_id);
        Terms terms = client.ListManagementTerm.GetAllTerms(list_id, lang);
        TermsData data = terms.Data;
        foreach (TermsInList term in data.Terms)
        {
        	Console.WriteLine(term.Term);
        }
        Thread.Sleep(throttleRate);
	}

## Add code to refresh the search index

After you make changes to a term list, you refresh its search index for the changes to be included the next time you use the term list to screen text. This is similar to how a search engine on your desktop (if enabled) or a web search engine continually refreshes its index to include new files or pages.

You refresh a term list search index with **ContentModeratorClient.ListManagementTermLists.RefreshIndexMethod**.

Add the following method definition to namespace TermLists, class Program.

	/// <summary>
	/// Refresh the search index for the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list to refresh.</param>
    static void RefreshSearchIndex (ContentModeratorClient client, string list_id)
    {
    	Console.WriteLine("Refreshing search index for term list with ID {0}.", list_id);
        client.ListManagementTermLists.RefreshIndexMethod(list_id, lang);
        Thread.Sleep((int)(latencyDelay * 60 * 1000));
	}

## Screen text using a term list

You screen text using a term list with **ContentModeratorClient.TextModeration.ScreenText**, which takes the following parameters.

- The language of the terms in the term list.
- A MIME type, which can be "text/html", "text/xml", "text/markdown", or "text/plain".
- The text to screen.
- A boolean value. Set this field to **true** to autocorrect the text before screening it.
- A boolean value. Set this field to **true** to detect Personal Identifiable Information (PII) in the text.
- The term list ID.

For more information, see the [API reference](https://westus2.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f).

**ScreenText** returns a **Screen** object, which has a **Terms** property that lists any terms that Content Moderator detected in the screening. Note that if Content Moderator did not detect any terms during the screening, the **Terms** property has value **null**.

Add the following method definition to namespace TermLists, class Program.

	/// <summary>
	/// Screen the indicated text for terms in the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list to use to screen the text.</param>
    /// <param name="text">The text to screen.</param>
    static void ScreenText (ContentModeratorClient client, string list_id, string text)
    {
    	Console.WriteLine("Screening text: \"{0}\" using term list with ID {1}.", text, list_id);
        Screen screen = client.TextModeration.ScreenText(lang, "text/plain", text, false, false, list_id);
        if (null == screen.Terms)
        {
        	Console.WriteLine("No terms from the term list were detected in the text.");
        }
        else
        {
        	foreach (DetectedTerms term in screen.Terms)
            {
            	Console.WriteLine(String.Format("Found term: \"{0}\" from list ID {1} at index {2}.", term.Term, term.ListId, term.Index));
            }
        }
        read.Sleep(throttleRate);
	}

## Delete terms and lists

Deleting a term or a list is straightforward. You use the SDK to do the following tasks:

- Delete a term. (**ContentModeratorClient.ListManagementTerm.DeleteTerm**)
- Delete all the terms in a list without deleting the list. (**ContentModeratorClient.ListManagementTerm.DeleteAllTerms**)
- Delete a list and all of its contents. (**ContentModeratorClient.ListManagementTermLists.Delete**)

### Delete a term

Add the following method definition to namespace TermLists, class Program.

	/// <summary>
    /// Delete a term from the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list from which to delete the term.</param>
    /// <param name="term">The term to delete.</param>
    static void DeleteTerm (ContentModeratorClient client, string list_id, string term)
    {
    	Console.WriteLine("Removed term \"{0}\" from term list with ID {1}.", term, list_id);
        client.ListManagementTerm.DeleteTerm(list_id, term, lang);
        Thread.Sleep(throttleRate);
	}

### Delete all terms in a term list

Add the following method definition to namespace TermLists, class Program.

	/// <summary>
    /// Delete all terms from the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list from which to delete all terms.</param>
    static void DeleteAllTerms (ContentModeratorClient client, string list_id)
    {
    	Console.WriteLine("Removing all terms from term list with ID {0}.", list_id);
        client.ListManagementTerm.DeleteAllTerms(list_id, lang);
        Thread.Sleep(throttleRate);
    }

### Delete a term list

Add the following method definition to namespace TermLists, class Program.

	/// <summary>
    /// Delete the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list to delete.</param>
    static void DeleteTermList (ContentModeratorClient client, string list_id)
    {
	    Console.WriteLine("Deleting term list with ID {0}.", list_id);
        client.ListManagementTermLists.Delete(list_id);
        Thread.Sleep(throttleRate);
    }

## Putting it all together

Add the **Main** method definition to namespace TermLists, class Program. Finally, close the Program class and the TermLists namespace.

	static void Main(string[] args)
    {
    	using (var client = Clients.NewClient())
        {
        	string list_id = CreateTermList(client);

            UpdateTermList(client, list_id, "name", "description");
            AddTerm(client, list_id, "term1");
            AddTerm(client, list_id, "term2");

            GetAllTerms(client, list_id);

            // Always remember to refresh the search index of your list
            RefreshSearchIndex(client, list_id);

            string text = "This text contains the terms \"term1\" and \"term2\".";
            ScreenText(client, list_id, text);

            DeleteTerm(client, list_id, "term1");

            // Always remember to refresh the search index of your list
            RefreshSearchIndex(client, list_id);

            text = "This text contains the terms \"term1\" and \"term2\".";
            ScreenText(client, list_id, text);

            DeleteAllTerms(client, list_id);
            DeleteTermList(client, list_id);

            Console.WriteLine("Press ENTER to close the application.");
            Console.ReadLine();
        }
	}

## Run the application to see the output

Your output will be on the following lines but the data may vary.

	Creating term list.
	Term list created. ID: 252.
	Updating information for term list with ID 252.
	
	Adding term "term1" to term list with ID 252.
	Adding term "term2" to term list with ID 252.
	
	Getting terms in term list with ID 252.
	term1
	term2
	
	Refreshing search index for term list with ID 252.
	
	Screening text: "This text contains the terms "term1" and "term2"." using term list with ID 252.
	Found term: "term1" from list ID 252 at index 32.
	Found term: "term2" from list ID 252 at index 46.
	
	Removed term "term1" from term list with ID 252.
	
	Refreshing search index for term list with ID 252.
	
	Screening text: "This text contains the terms "term1" and "term2"." using term list with ID 252.
	Found term: "term2" from list ID 252 at index 46.
	
	Removing all terms from term list with ID 252.
	Deleting term list with ID 252.
	Press ENTER to close the application.
	
## Next steps

[Download the Visual Studio solution](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) for this and other Content Moderator quickstarts for .NET, and get started on your integration.
