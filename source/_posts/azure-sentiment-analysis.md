---
title: Azure Sentiment Analytics - How Negative Am I?
categories:
 - [Azure, Cognitive Services, Sentiment Analytics]
 - [Azure, Cognitive Services, Text Analytics]
date: 2018-07-11 16:22:01
tags:
 - Azure
 - Azure Functions
 - Azure Cognitive Services
 - Azure Sentiment Analytics
 - Personal Growth
---
Recently I was given the feedback that I am quite negative in the office. I was not surprised by this feedback as I have noticed it in myself too, and I want to do something about it.

I have a goal to be more positive in 12 months time, but the problem with that goal is that it is pretty hard to measure, it will just be about perspective of others around me without any "concrete" proof.

In order to have some fact behind whether or not I achieve my goal, I going to attempt to use Azure Sentiment Analysis on my social media and emails for the next 12 months.

In this post, I discuss what I am intending to measure and what I have set up with Azure to help me measure it.
<!-- more --> 

## What is Azure Cognitive Services - Text Analytics

Azure Cognitive Services - Text Analytics will take in text, analyse the words in the text and give a sentiment rating on that text. The sentiment rating is just a scale from 0 (negative) to 100 (positive). If sentiment can't be detected on the text, or it is not very confident in the sentiment, the rating will be close to the middle (~50).

You can try it out live on the [Azure website](https://azure.microsoft.com/en-us/services/cognitive-services/text-analytics/?v=18.05).

If you enter something with no clear positive or negative sentiment you will get a neutral score.
e.g. "The Flyers score on the power play" gives a sentiment score of exactly 50. If I enter "Claude Giroux's power play goal was amazing" it gives a sentiment score of 93. Finally; if I enter "The Penguins are the worst team to follow in the NHL" I get a sentiment score of 20.

The sentiment analysis also picks up that I am talking about the Pittsburgh Penguins and the National Hockey League:

{% asset_img "example-negative-input.png" "Example - Negative Input"%}

The API for this result gives you a strucuted JSON output:

```json
{
  "languageDetection": {
    "documents": [
      {
        "id": "aa7bedff-1e09-4ff1-8751-661c977bf684",
        "detectedLanguages": [
          {
            "name": "English",
            "iso6391Name": "en",
            "score": 1.0
          }
        ]
      }
    ],
    "errors": []
  },
  "keyPhrases": {
    "documents": [
      {
        "id": "aa7bedff-1e09-4ff1-8751-661c977bf684",
        "keyPhrases": [
          "worst team",
          "Penguins"
        ]
      }
    ],
    "errors": []
  },
  "sentiment": {
    "documents": [
      {
        "id": "aa7bedff-1e09-4ff1-8751-661c977bf684",
        "score": 0.19182384014129639
      }
    ],
    "errors": []
  },
  "entities": {
    "documents": [
      {
        "id": "aa7bedff-1e09-4ff1-8751-661c977bf684",
        "entities": [
          {
            "name": "Pittsburgh Penguins",
            "matches": [
              {
                "text": "Penguins",
                "offset": 4,
                "length": 8
              }
            ],
            "wikipediaLanguage": "en",
            "wikipediaId": "Pittsburgh Penguins",
            "wikipediaUrl": "https://en.wikipedia.org/wiki/Pittsburgh_Penguins",
            "bingId": "6bba894c-8bbc-aab5-6169-b5797b07de04"
          },
          {
            "name": "National Hockey League",
            "matches": [
              {
                "text": "the NHL",
                "offset": 45,
                "length": 7
              }
            ],
            "wikipediaLanguage": "en",
            "wikipediaId": "National Hockey League",
            "wikipediaUrl": "https://en.wikipedia.org/wiki/National_Hockey_League",
            "bingId": "f57d616b-b94b-8ea4-037c-0d51371d3083"
          }
        ]
      }
    ],
    "errors": []
  }
}

```

## Using Cognitive Services - Text Analytics

To get started with this, I first created a Text Analytics resources in Azure.

{% asset_img "azure-create-text-analytics.png" "Create Text Analytics Resource"%}

To do this; open your Azure portal and follow the following:
1. Click "Create a resource"
1. Select "AI + Machine Learning"
1. Select "Text Analytics"
1. Enter a name for your new Text Analytics resource
1. Choose your subscription for it to be created in
1. Choose a location for your resource to be hosted in
1. Choose your pricing tier (F0 is the free tier with 5000 transactions per month)
1. Create a new resource group, or use an existing one

And then you can save to create your new Text Analytics resource.

You now have a Text Analytics API that you are able to call. In order to use it you need to get your Keys, and find your API endpoint.

You can find these under the Overview section, and the Keys sections on your resource.

You can contact your Text Analytics endpoint with a basic Http Client like the following:

```csharp
  HttpClient client = new HttpClient();
  client.BaseAddress = new Uri("https://YOURLOCATIONHERE.api.cognitive.microsoft.com/text/analytics/v2.0/");

  client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", "YOUR KEY HERE");

  var result = await client.PostAsJsonAsync("sentiment",
      new SentimentRequest
      {
          Documents = new List<DocumentRequest>
          {
              new DocumentRequest
              {
                  Id = "1",
                  Language = "en",
                  Text = "The Philadelphia Flyers prospects will shine this year"
              }
          }
      });

  var sentimentResult = JObject.Parse(await result.Content.ReadAsStringAsync()).ToObject<SentimentResult>();
```
The code above assumes some basic classes in place to strongly type the data when sending and receiving like the following:

```csharp

public class DocumentRequest
{
    [JsonProperty("language")]
    public string Language { get; set; }
    [JsonProperty("id")]
    public string Id { get; set; }
    [JsonProperty("text")]
    public string Text { get; set; }
}

public class DocumentResult
{
    [JsonProperty("id")]
    public string Id { get; set; }
    [JsonProperty("score")]
    public double Score { get; set; }
}

public class SentimentRequest
{
    [JsonProperty("documents")]
    public IList<DocumentRequest> Documents { get; set; }
}

public class SentimentResult
{
    [JsonProperty("documents")]
    public IList<DocumentResult> Documents { get; set; }
}

```

## Sending my data to Text Analytics

I am not normally very vocal on social media, so I am going to make an effor to post more in general, especially when I feel strongly about issues or topics. I also send a lot of emails, I have not figured out how I will get my emails into the sentiment detection automatically yet. There are Office365 API's available but it looks like that you need to add an application to your AzureAD in order to use them, which the company I work for will likely not allow me to do.

At this stage I will likely use [Postman](https://www.getpostman.com/) to manually put my emails through sentiment analytics. If I figure out an automated way to achieve this; I will write a follow up post with my method.

There is a limit to how much text the Text Analytics service can process at once, that limit is 5000 characters. I will just skip any social media post or email that is larger than that limit as splitting it up may skew the sentiment.

I need to set a baseline for how negative I was in the past year in order to figure out if there has been any noticable improvement when I have tried to be positive for the next year. To do this; I decided that I would pull the last years posts from Twitter, Facebook, and Reddit and store them in an Azure Table. I will then automate pulling in future posts on these mediums and save them to the same table. After a year is up; I will analyse the data and confirm if I was able to be more positive or not.

The data I am storing from each medium is the following:
- Medium (Facebook, Twitter, Reddit [I may add email at a later stage])
- Date & Time (The date and time that I made the content)
- RawContent (The content that I posted)
- SentimentRating (The sentiment rating generated from Azure)
- SentimentJSON (The JSON output from the sentiment result)

The code I am using is a series of Azure Functions to pull the data from the various sources and save them to the Azure table. In order to run this code in Azure you will need a Function App, and a storage account to save it to. You can view the code on my [GitHub](https://github.com/Brendon-Coombes/SentimentAnalytics). This is not structured particularly well, it was just thrown together for the purpose of retrieving the data.

Facebook's API was a little too restrictive with the content that the Graph API would output, so instead of having an automated function to pull in data from the API, I have a monthly reminder to upload my data manually. You can see how to get your data from Facebook on the [Facebook help pages](https://www.facebook.com/help/212802592074644/?ref=u2u). I downloaded my data in JSON format, and sent the comments/comments.json, and posts/your_posts.json files to my endpoint with a specified date range.

## The Baseline

I have used the code above to set a baseline for how negative I was over the past 12 months and found the following results:

- I used the Twitter API to pull my posts from July 2017 until July 2018 and found my average sentiment was:
- I used the Facebook API to pull my posts from July 2017 until July 2018 and found my average sentiment was:
- I used the Reddit API to pull my posts from July 2017 until July 2018 and found my average sentiment was:

This shows that I am more negative on twitter etc, don't know how bad other people are compared to this? Does x represent a negative person?

I have not written any code to pull my emails back yet, but will add it in here if I do that in the future.
