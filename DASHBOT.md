# Steps to create a Dashbot account

This is a two step process, one part in the browser and one part in the source code. Make sure to follow Dashbot instructions for your language/platform!

## Create your bot

1. Visit https://www.dashbot.io/ and click "Sign Up".

2. Create a new Bot by clicking "Add a Bot, Skill, or Action".

3. For Platform select Generic (for NodeJS app running locally) or Slack. Give you bot a name, and optionally select a category, add a URL (if deploying it), and an Image URL if that's interesting to you. Click "Save Bot".

4. Your bot should now be listed in the dashboard. Click "Edit" to find your API key. Click the arrow key to see the reports dashboard.

## Add code to feed bot

I did this for you, but checkout [app.js](./app.js).

![alt text][post]

dashbot.logIncoming -> User typed messages

![alt text][updateMessage]

dashbot.logOutgoing -> Watson responses

Message text and UserId required (userID must the same to communicate in the same channel)


[updateMessage]: ./readme_images/updateMessage_dashbot.png "alt text"
[post]: ./readme_images/post_dashbot.png "alt text"
