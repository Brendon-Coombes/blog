---
title: Dynamics 365 Retrieve Multiple - Unknown Condition Operator:DoesNotContain
categories:
 - Development
date: 2018-02-27 06:15:24
tags:
 - Development
 - Dynamics365
 - CRM
---

Today; I was making a small console app to anonymise the data we had in our development CRM environment and ran into an unexpected issue. I was attempting to pull in some contact data from CRM that needed to match some basic criteria: "Last Name Does Not Contain 'Service'". I did a basic retrieve multiple query and got the error: "Unknown Condition Operator: DoesNotContain". 
<!-- more --> 

We have some contacts set up that are used for service tasks, and did not want to change the data in those contact records, but needed to change them in the rest.

All my console app did was pull in records, and then change the first name, last name, email address, and phone number (that is all of the information we had on each contact).

The retrieve code I was using was pretty simple:

```csharp
public IEnumerable<Entity> GetAllContacts()
{
    var query = new QueryExpression()
    {
        EntityName = "contact",
        ColumnSet = new ColumnSet("firstname", "lastname", "telephone1", "emailaddress1")
    };

    //Don't update service accounts and users we know are using the system
    query.Criteria.Conditions.Add(new ConditionExpression("lastname", ConditionOperator.DoesNotContain, "Service"));

    var results = base.RetrieveMultiple(query);
    return results.Entities;
}
```

However, when I attempted to run this code, I would get an exception:
```
System.ServiceModel.FaultException`1: 'Unknown Condition Operator: DoesNotContain'
```

I was sure you can do this in Advanced Find within CRM, so I didn't understand why it did not work in code.

I created the query in Advanced Find to prove that the CRM must execute the fetch in a similar way:

{% asset_img "advanced-find-example.png" "Advanced Find example"%}

The query executed as expected and gave me the records I was looking for. So I then downloaded the fetch xml to see if I could see anything that would give me a clue, which is where I discovered my problem:

```xml
<fetch version="1.0" output-format="xml-platform" mapping="logical" distinct="false">
    <entity name="contact">
        <attribute name="contactid" />
        <attribute name="firstname" />
        <attribute name="lastname" />
        <attribute name="telephone1" />    
        <attribute name="emailaddress1" />
        <order attribute="firstname" descending="false" />
        <filter type="and">
            <condition attribute="lastname" operator="not-like" value="%Service%" />
        </filter>
    </entity>
</fetch>
```
The operator that was inserted into the fetch xml was not "DoesNotContain", but instead it used "not-like' and wrapped my value with wildcards '%'.

I changed my code to use the same operator and wrapped my value with wildcards, and my query now executes successfully.

```csharp
public IEnumerable<Entity> GetAllContacts()
{
    var query = new QueryExpression()
    {
        EntityName = "contact",
        ColumnSet = new ColumnSet("firstname", "lastname", "telephone1", "emailaddress1")
    };

    //Don't update service accounts and users we know are using the system
    //User NotLike rather than DoesNotContain - DoesNotContain will throw an exception.
    query.Criteria.Conditions.Add(new ConditionExpression("lastname", ConditionOperator.NotLike, "%Service%"));

    var results = base.RetrieveMultiple(query);
    return results.Entities;
}
```