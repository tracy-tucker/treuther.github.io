---
layout: post
title:      "CLI Gem Portfolio Project"
date:       2019-08-03 16:11:15 -0400
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
* Create a collection of objects, not hashes, to store data.

## The Site
![](https://res.cloudinary.com/tracyr/image/upload/v1564861020/coffeeicon_d5fial.jpg)

My original plan was to scrape a website that listed the 13 best coffee shops in my area that had great work environments. That didn't really pan out because the "level-deep" information on each coffee shop required scraping multiple external sites. So, instead I decided to scrape a website that sells coffee. I love coffee so why not? The website had many menu options to choose from, so I decided to stick with the "Manager's Special" list to scrape for data.

## The Pre-Scrape
![](https://res.cloudinary.com/tracyr/image/upload/v1564861299/pre-scrape-example_kkkvd6.jpg)

In order to pre-screen a website to see if it is in fact "scrapable", it's a good a idea to go ahead and scrape it for the data that you are trying to collect.

The Nokogiri gem is the perfect tool for the job. After a little bit of scraping, I discovered the three main elements and CSS selectors I needed in order to grab the correct data:

```
("div.card-body")
("span.price.price--non-sale")
("span.price.price--withoutTax")
```

After affirmation that the data was "scrapable", I saved the work and turned my focus to setting up the environment for the CLI application.

## The Setup
![](https://res.cloudinary.com/tracyr/image/upload/v1564861998/file-structure_ynivba.jpg)

To setup up the environment, I ran the following code in the terminal to get started:

`bundle gem [project_name]` and then followed the prompts.
>**- The most important thing I needed to remember is to make sure the application had executable permissions for the user -**

I typed the following in the terminal: `ls -lah` to see a list of permissions that are in place for the CLI application.

Then, I typed `chmod +x [project_name]` into the terminal to set the application to both "readable" and "writable" permissions in order for the user to execute commands for the CLI application.

After the entirety of the initial setup, I was ready to go with a solid file structure for my project


Now that the file structure is complete, I needed to set up the project's environment for open communication between files. First, I needed to add the following to the environments.rb file:

```
require_relative "./coffee_sale/version"
require_relative "./coffee_sale/cli"
require_relative "./coffee_sale/coffee"
require_relative "./coffee_sale/scraper"

require 'pry'
require 'nokogiri'
require 'open-uri'
require 'colorize'
```
In order for each file to communicate with one another, I needed to **require** all created files. I also needed to setup `binding.pry` in order to test the working code in the terminal. I also established `nokogiri`, `open-uri` and `colorize`. 

Nokogiri is a parser that parses and searches (or scrapes) XML/HTML using libraries. It allows you to search documents using XPATH and CSS selectors to obtain desired information.
Open_URI is a wrapper that makes it possible to open http or https in your application as if it were a file, which is very convenient.
Colorize is a neat string class extension that will add color to the command line output, which creates a more readable for the user (just my opinion).

## The CLI Environment
![](https://res.cloudinary.com/tracyr/image/upload/v1564862347/cli-screenshot_nw3tkh.jpg)

**BEFORE GOING ANY FURTHER** I needed to do the following - I needed to assign a class to the file **AND** associate (or use namespacing) the newly created class with the project gem.

This basically tells the class that it belongs to the project gem, and we do this because there are many gems that can be associated with one project and we want **THIS** class to know which gem it belongs to.
```
class CoffeeSale::CLI
```
And I started every \rb file I created and used namespacing for the class setup.

At first, it was difficult for me to wrap my head around the idea of creating a "fake" CLI environment instead of just going *straight for the scrape*. But taking note of others starting in the CLI file made me decide to follow suit - **and it was an excellent idea.** After digging in, it made total sense to do this first.

I broke this down in baby-steps:
* Get the coffees
* List the coffees
* Get the user's next selection
* Get their next answer (continue or no?)
* Or say goodbye

While building the draft of the cli.rb, I did have to put in some "fake" data in the scraper.rb in order  to test the application and see if my methods were working properly.

After I felt pretty solid on this, I moved to the coffee.rb file.
## Coffee.rb

![](https://res.cloudinary.com/tracyr/image/upload/v1564862539/coffee_screenshot_f7dgcu.png)

This file's sole purpose is to be the point of communication between the cli.rb file and the scraper.rb file. The cli shouldn't need to communicate with the scraper. Thus the creation of the coffee.rb.

I needed to initialize a new instance of a coffee and pass-in the 3 arguments from the data that I was going to pull from the scraper.
* Name
* Original price
* Sale price

Which led me to have an attribute reader and a writer for each: `attr_accessor`.

Not only did I want to instantiate a new coffee, I also wanted to **store** and **save** that new instance in a class variable, which is assigned to an empty array.

And that's the jist of the coffee.rb file. It creates the instance, then stores it!

## The Scraper
![](https://res.cloudinary.com/tracyr/image/upload/v1564862731/scraper-screenshot_ollghq.jpg)

Last, but not least, it was time to tackle the scraper. I already accomplished the hard part of finding which CSS selectors to utilize in my pre-scrape in order to grab the desired data. Now, I just needed to copy that info over to the scraper.rb. I did need to add a way to push the scraped information into a new instance of Coffee.

```
CoffeeSale::Coffee.new(name, orig_price, sale_price)
```
[[And, of course, all the while I used the pry gem to test my code along the way. I needed to make sure I was creating a collection of objects and relationships with the ability to communicate with one another.

## Summary

Overall, this project was pretty challenging for me - I'm not going to lie. And I think it's because it was taking me away from the lab environment and away from an rspec that would give me "clues" as to what to code next. I had to think for myself and on my own. I honestly wanted to ellaporate a lot more on this project by adding more to the description section. But that would have involved me going much deeper in my code development and I honestly couldn't handle pulling out the remaining strans of my hair :-)
I know some would say that I'm cutting myself too short on this project. That I **should** have went further with it... and honestly I probably should have. I'm hoping to have more time in the near future utilizing everything I learned in Procedural Ruby and OO Ruby.

## The Video

Enjoy my "techy" explanation video. I honestly did the best I could explaining the project in the video. I'm sure I'll get better in time and have a stronger presentation in the next one!

**[YOUTUBE VIDEO GOES HERE]**


