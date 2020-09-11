---
layout: post
title: "Salesforce Custom Metadata vs. Custom Objects"
date: 2018-10-18 11:00
redirect_from: '/FptM'
tags: ['salesforce','custom metadata','tutorials']
lang: en
otherlang-slug: salesforce-metadonnees-personnalisees-v-objets-personnalises
---

A colleague, new to Salesforce, asked me the difference between "**custom metadata**" and "**custom objects**" for storing "configuration and validation" information in Salesforce.

My answer is:

> * **[Custom metadata](https://www.youtube.com/watch?v=kFwwcLxkkjI)**{:target="_blank"} if you can. Salesforce says so.  <br/>*(And it survives sandbox refreshes!)*
> 
> * **Data tables (custom objects)** if you truly need it to be part of your data <br/>_(e.g. people will want to include it, hyperlinked to other data, in reports)_

## How I decided

When I wrote an Apex trigger that determined which User should "own" each "Admissions Application" in our org, I ended up splitting the configuration data in two.

Here's why.

### Custom Metadata

Used for a table that helps Apex code ask, "Who does this?"

* key:  the code for a graduate degree we offer
* value: a username responsible for recruiting people to that graduate degree

### Data (Custom Objects)

Used for a list of U.S. states, their full spellings, and fields with "lookup" links to the User table indicating who does what work for that state.

* There was strong demand to generate native Salesforce reports per User showing all the states they're responsible for and the high schools in those states.  It made sense to ensure that the "high school" table could have a "lookup" field to this "states" table.
* Custom metadata can have "master-detail" and "lookup" relational links to other custom metadata, but it **can't** link to ordinary data.
  * This meant we needed to store the the "states" as data (custom objects), even though we would also be using it as configuration information for Apex triggers.

## UI & editability considerations

I'll let you in on a dirty little secret about _another_ reason I used **data** tables ("custom objects") for most of the Undergraduate Admissions counselor assignment configuration data.

Undergraduate Admissions tweaks their "recruiter assignment" business rules several times a year. The real-world configuration data for their business is a _lot_ more complex than a simple "list of U.S. states."

I'll be honest: Salesforce's user interfaces for hand-editing, viewing, and bulk-editing **data** are a lot more end-user-friendly than their user interfaces for the same operations on **custom metadata**, and setting granular "edit" permissions is a lot more sysadmin-friendly for data. I wanted to make sure end users, not sysadmins, were the ones whose time was spent tweaking the configuration several times a year!

I was thoroughly scolded at Dreamforce. Actually, I stand by my decision to use "data" tables, because there truly is a business need to report on the configuration data alongside normal data. But ... building your own user interfaces _(typically using Lightning Components)_ to help end users edit custom metadata was a big theme. You have been warned:

1.  [Dan Appleman's "Build Awesome Configuration Pages with Lightning Components & Custom Metadata"](https://www.youtube.com/watch?v=Qr2tqjWnXgY){:target="_blank"}
2.  [Gustavo Melendez & Krystian Charubin's "Crafting Flexible APIs in Apex Using Custom Metadata"](https://www.youtube.com/watch?v=sGhAcGjQzyo){:target="_blank"}
3.  [Beth Breisness & Randi Wilson's "Create Guided User Experiences for Managing ISV Custom Metadata"](https://www.youtube.com/watch?v=nlqFB89DhfI){:target="_blank"}
