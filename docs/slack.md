# Slack integration with swagger-node

[Slack](https://slack.com/) is a messaging app for team communication. A nice thing about Slack is that you can easily integrate external services to provide extra features, like posting tweets or accessing Google docs. 

## About the sample app

Turns out that it's easy to create a [swagger-node](https://www.npmjs.com/package/swagger) app that integrates with Slack. We have two sample implementations that you can try out:

* A Slack slash command that reverses whatever text you enter. Silly, but it works!

* An Incoming WebHook command that fetches a stock quote and prints it to a specified Slack channel. 


## Get the sample

Download or clone the [swagger-node-slack](https://github.com/apigee-127/swagger-node-slack) project on GitHub. 


## Text reverser

Here's how it works. Enter this slash command into Slack:

`/reverse Hello World`

And you get a reply with the characters reversed:

![alt text](./images/reverse.png)

## Stock ticker

In Slack, enter:

`ticker AAPL`

You get back a nicely formatted response, like this:

![alt text](./images/stockbot.png)

## Making a Slack Slash Command integration with A-127

Let's walk through the steps to create a Slak integration using swagger-node as the back-end. We're not going to go overboard to explain how to set things up in Slack, but we'll give pointers to keep you on track. It's remarkably easy.

### Before you begin

1. First, you need to either be a member of or create new Slack team. Go to [slack.com](slack.com) for details. 
2. Your app has to be reachable by Slack, and it must be deployed to a platform that supports Node.js. We're going to deploy it to Apigee Edge. So, to do that, you'll need to [sign up for an Apigee Account](https://accounts.apigee.com/accounts/sign_up). 

### Build it

1. From your Slack team menu, choose **Configure Integrations**.
2. Click **Slash Commands**. 
3. In Choose Commands, enter the command name `/reverse`. 
3. Okay, now fill out the integration settings:
    a. Command:  `/reverse`. 
    b. URL: http://{your apigee org name}-{the apigee environment name}.apigee.net/slack/reverse

    For example: http://docs-test.apigee.net/slack/reverse

    c. Method: POST
    d. Token: You'll need this later.
4. Click **Save Integration**. 

![alt text](./images/reverse-done.png)

### Publish it

For this example, we'll publish the app to the [Apigee Edge](http://www.apigee.com) platform. You can publish it to any platform that supports Node.js. 

1. Install [apigeetool](https://www.npmjs.com/package/apigeetool). This utility let you deploy Node.js apps to Apigee Edge. 

    `npm -g apigeetool`

2. Make sure `apigeetool` is in your PATH. Just enter `apigeetool` at the command line. If you see a list of valid commands, you're good to go. 

    `apigeetool`

3. Be sure you're in the root directory of the `swagger-node` project:

    `cd <my swagger-node project>`

4. Use `apigeetool` to deploy the app:

    `apigeetool deploynodeapp -u <your Apigee account email address> -o <your Apigee organization name> -e <An environment in your org> -n 'slash' -d . -m app.js -b /slash`

    For example:

    `apigeetool deploynodeapp -u jdoe@apigee.com -o jdoe -e test -n 'slash' -d . -m app.js -b /slash`

    For help on this command, enter `apigeetool deploynodeapp -h`.

## Test it

Now, what we have is a backend target app that the Slack integration we created can hit. All it does is reverse whatever text you enter when you execute it in a Slack chat session. But you get the idea -- you could program the `swagger-node app` to do whatever you wish. 

In your Slack channel, enter something like this:

`/reverse The quick brown fox jumps over the lazy dog`

![alt text](./images/quickfox-1.png)

And Slack returns the letters in reverse:

![alt text](./images/quickfox-2.png)

## What happened?

The Slack Slash Command Integration called the `swagger-node` app that was deployed on Apigee Edge. Slack retrieved the response and printed it to the chat window. 









