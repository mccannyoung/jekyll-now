---
layout: post
title: Machine Learning Toy Application
---

Just to change gears a bit, I've decided to apply my new go skills to a machine learning problem. Now I took Artificial Intelligence and Machine Learning in graduate school, but that was five years ago and I haven't had occasion to use it for work since. 

[Write more about machine learning in general](http://www.cs.cmu.edu/~tom/pubs/MachineLearning.pdf)

[This is a useful article](http://homes.cs.washington.edu/~pedrod/papers/cacm12.pdf)

A very minor product at my work is a call-in line for a manufacturer for the citizens who live by their plants.  They also have a line people can call to hear announcements, about current events. It would be useful if there was a way to categorize call ins and flag the ones about not-known issues, particularly calls about severe events. 

Because this is a toy example, let's limit the scope of possible events to four things:

- Fire
- Earthquake
- Tsunami
- Zombies

Let's have the call-in announcement line be controlled via a website. We're going to use Twilio to handle the backend phone system. Twilio has a [wonderfully documented programmer-friendly API](https://www.twilio.com/docs/). Twilio has a lot of libraries for various languages, it has nothing for go, the language I'll be using in this, but because all we're going to need to do is generate TwiML. 

For machine learning, we'll just explore the [AWS machine learning](http://docs.aws.amazon.com/machine-learning/latest/dg/machine-learning-concepts.html) service. 

