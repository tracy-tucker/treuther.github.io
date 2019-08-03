---
layout: post
title:      "CLI Gem Portfolio Project"
date:       2019-08-03 20:11:14 +0000
permalink:  cli_gem_portfolio_project
---

![](https://res.cloudinary.com/tracyr/image/upload/v1564778446/CoffeeIcon_lwjll5.jpg)

### One Down - A Few More to Go

I made it to the first project of the Full Stack Development self-paced course at Flatiron School. As a student, we spend all our time reading each lesson and passing lab-after-lab. When it's time to put all the puzzle pieces together, it can feel a little overwhelming - which is **exactly** how I felt as soon as I started reading all the requirements, tips and "dos and don'ts".

>"I'm going to go stand in traffic now"

I'm pretty sure that's what I said out loud the moment I was finished taking everything in. Nonetheless! Here I am. The first project under my belt and I can say that I'm looking forward to the next one.

## The Project Low-Down

The logistics of the project are as follows:

* Provide a CLI (command line interface) application that gives access to data from a web page
* The data must go a least one level deep to where a user can make a selection and then see more information about their selection
* The CLI must not be too similar to listed labs: Kickstarter, Student Scraper, TTT with AI  and Music CLI
* Create a collection of objects, not hashes to store data.

## The Site
![](https://res.cloudinary.com/tracyr/image/upload/v1564861020/coffeeicon_d5fial.jpg)

My original plan was to scrape a website that listed the 13 best coffee shops in my area that had great work environments. That didn't really pan out because the "level-deep" information on each coffee shop required scraping multiple external sites. So, instead I decided to scrape a website that sells coffee. I love coffee so why not? The website had many menu options to choose from, so I decided to stick with the "Manager's Special" list to scrape for data.

## The Pre-Scrape
![](https://res.cloudinary.com/tracyr/image/upload/v1564861299/pre-scrape-example_kkkvd6.jpg)

In order to pre-screen a website to see if it is in fact "scrapable", it's a good a idea to go ahead and scrape it for the data that you are trying to collect.

The Nokogiri gem is the perfect tool for the job. After a little bit of scraping, I discovered the three main elements and CSS selectors I needed in order to grab the correct data:

```
doc.css("div.card-body")
c.css('span.price.price--non-sale')
c.css('span.price.price--withoutTax')
```

After affirmation that the data was "scrapable", I saved the work and turned my focus to setting up the environment for the CLI application.

## The Setup
![](https://res.cloudinary.com/tracyr/image/upload/v1564861998/file-structure_ynivba.jpg)

To setup up the environment, I ran the following code in the terminal to get started:

`bundle gem [project_name]` and then followed the prompts.
The most important thing I needed to remember is to **make sure the application has executable permissions**

I typed the following in the terminal: `ls -lah` to see a list of permissions that are in place for the CLI application.

Then, I typed `chmod +x [project_name]` into the terminal to set the application as both readable and writable in order for the user to make executions to the CLI application.

After the entirety of the initial setup, I was ready to go with a solid file structure for my project



[In order create for my files to communicate with one another, I needed to build the connection between files in my environments.rb file:]


START HERE

## The CLI Environment
![](https://res.cloudinary.com/tracyr/image/upload/v1564862347/cli-screenshot_nw3tkh.jpg)

put information here

## Coffee.rb

![](https://res.cloudinary.com/tracyr/image/upload/v1564862539/coffee_screenshot_f7dgcu.png)

put information here

## The Scraper
![](https://res.cloudinary.com/tracyr/image/upload/v1564862731/scraper-screenshot_ollghq.jpg)

put information here

## Summary

put information here

## The Video

Enjoy my explanation video. I'm not claiming to be any type of export by any means. I honestly did the best I could explaining the project in the video. I'm sure I'll be better in time for the next one!

**[YOUTUBE VIDEO GOES HERE]**


I honestly wanted to ellaporate a lot more on this project by adding more to the description section. 
