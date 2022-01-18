# 7. App Redesign

The app we had been envisioning so far had several disadvantages:

- The app concept is general (i.e. encouraging people to see doctors based on their health data), but the client’s focus is very specific — the client is specifically focused on COPD.
- The premise of writing sub-apps for each health test is inelegant — it’s far simpler to implement a general purpose symptom checker such as a medical chatbot.
- The design is too similar to Apple’s existing health app.

As such, we underwent a major redesign of the app, making it centre more around a symptom checker chat bot. Chat bots have been historically very successful in this area, and it’s to some extent a solved problem. It also provides a standard format for users to enter data and receive feedback. Our app however will have an additional feature of providing forecasts and insightful visualisations of their health progress. 

New prototype inspired by Ada:

![Untitled](/assets/7%20App%20Redesign%201a31847f58bb4c3993dcc0d90aa08452/Untitled%201.png)

This is a far simpler design but offers much more functionality due to being general purpose.

This page is intended to show visualisations and insights about the user’s health progress. The more-menu will provide options such as “clear data”, “export data”, and “dark mode”.

![Untitled](/assets/7%20App%20Redesign%201a31847f58bb4c3993dcc0d90aa0845/Untitled%202.png)

## UI Compression Again

Redesigning the app came at a small cost of having to recreate the minimal set of components we’ll need to implement. Since we haven’t started programming yet, this isn’t a major issue.

## New Development Approach

We also thought it would be wise to create two concurrent development forks. One will be implemented using our lightweight framework. The other will be implemented with Vue and Naive UI. The latter allows for fast development because Naive UI provides several dozen premade Vue components. However this is at the cost of efficiency. The former is our ideal application. Our approach will be to develop using Naive UI, and then translate finished pieces to our lightweight framework.