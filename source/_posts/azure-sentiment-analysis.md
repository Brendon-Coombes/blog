---
title: Azure Sentiment Analysis - How Negative Am I?
categories:
 - [Azure, Cognitive Services, Sentiment Analysis]
 - [Azure, Cognitive Services, Text Analytics]
date: 2018-07-11 16:22:01
tags:
 - Azure
 - Azure Cognitive Services
 - Azure Sentiment Analysis
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

Social Media, I am not very vocal on social media - so will make an effort to post more in general especially about how I am feeling about issues
Emails - talk about 5000 character limit - https://docs.microsoft.com/en-us/azure/cognitive-services/text-analytics/overview#supported-languages
Manual task at this point, will look into automating it on send on emails

Things I could have also looked at:
- Speech to text - 5 hours free per month.
