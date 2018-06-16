# AWS Hackathon: Creating an Alexa Skill

Today, we're going to divide up into teams, and each team is going to create their own Alexa Skill.  First, we'll walk you through the Alexa Skill creation process.  Then, each team will come up with their own idea for an Alexa skill, and work together to create it.

# First Alexa Skill

* **One person per team:** Create a new, shared amazon.com account for your team.  Do this at [the Amazon account registration page](https://www.amazon.com/ap/register?openid.pape.max_auth_age=0&openid.identity=http%3A%2F%2Fspecs.openid.net%2Fauth%2F2.0%2Fidentifier_select&pageId=usflex&ignoreAuthState=1&openid.return_to=https%3A%2F%2Fwww.amazon.com%2F%3F_encoding%3DUTF8%26ref_%3Dnav_ya_signin&prevRID=4CN21QYBVVZRQBNGEQZ7&openid.assoc_handle=usflex&openid.mode=checkid_setup&openid.ns.pape=http%3A%2F%2Fspecs.openid.net%2Fextensions%2Fpape%2F1.0&prepopulatedLoginId=&failedSignInCount=0&openid.claimed_id=http%3A%2F%2Fspecs.openid.net%2Fauth%2F2.0%2Fidentifier_select&openid.ns=http%3A%2F%2Fspecs.openid.net%2Fauth%2F2.0).

  * The **Email** (user id) can be something like `team777@aws-hackathon.io`, it does not have to exist
  * Ensure that each member of the team has the account's username and password

* **One person per team:** Go to https://developer.amazon.com and login with the account just created.   Click **Developer Console** in the upper right.  Fill out the information on the registration wizard.

  * You don't actually have to provide real values, e.g. your address can be `123 foobar court` if you want.
  * You don't plan to monetize your content/apps

* **Now that the account is set up, each team member can complete the below steps individually.**

* Create an Alexa Skill
  * Go to the Alexa Console https://developer.amazon.com/alexa/console/ask 
  * Click on the **Create Skill** button
  * Enter **First Skill by <YOUR_FIRST_NAME>** as the skill name and click **Next**
  * Select **Custom** and click the **Create skill** button
  * Click **JSON Editor** on the left bar
  * Paste the contents of [facts-skill-interaction-model.json](https://raw.githubusercontent.com/curtiseinsmann/alexa-hackathon/master/facts-skill-interaction-model.json) in the editor
  * Click the **Build Model** button (it will take a few minutes to build the model)
  * After the skill is done, click **Your Skills** to go back to the Alexa Console
  * Look for the **Skill ID**, which should be in a tiny font under your skill's title (e.g. `amzn1.ask.skill.5a1e8977-e4f4-4a74-ba4c-5badaf4ac849`) and take note of it -- we'll need this later.

* Sign in to the AWS Console
  * Open a new browser tab
  * Navigate to https://aws.amazon.com/ then click **Create AWS Account**, then click **Sign in to an existing AWS account**
  * Here's where things get a little tricky... the signin page looks slightly different, depending on whether or not you've been to the AWS console before.  We're going to be signing in as an **IAM user** specific to your team.  You'll need three pieces of information.
    * AWS Account ID: **186333523574**
    * IAM user name: `<provided to your team>`
    * IAM password: `<provided to your team>`
  * If you receive a password expiration notice, ignore it and continue.

* Create the corresponding Lambda Function in the AWS Console
  * Make sure that the upper right menu dropdown for region has **US East (N. Virginia)** selected
  * Type **Lambda** in the **AWS Services** box
  * Select the **Lambda** service
  * Click the **Create a function** button
  * Select **Blueprints**
  * Type the keyword **alexa** in the blueprints filter box
  * Click the **alexa-skill-kit-sdk-factskill** link blueprint
  * Type a *unique* **Name** like `alexaSkillTeam777`. Make sure the **Name** is unique enough, since you will see later the functions of all teams and you have to be able to recognize yours. Be careful *not* to edit another's team Lambda function by mistake.
  * For the **Role** dropdown, select the **Choose an existing role** option
  * For the **Existing Role** dropdown, select the **firstAlexaSkillRole** role option
  * Scroll down and click the **Create function** button
  * On the left, under **Add triggers** select the **Alexa Skills Kit** trigger
  * Scroll down and paste the **Skill ID** copied in previous section (e.g. `amzn1.ask.skill.5a1e8977-e4f4-4a74-ba4c-5badaf4ac849`)
  * Click the **Add** button
  * Scroll up and click the orange **Save** button
  * Copy the **ARN** on top of the orange **Save** button (e.g. `arn:aws:lambda:us-east-1:735534501522:function:alexaSkillTeam777`)
  

* Update the Endpoint of the Alexa Skill
  * Go back to the Alexa Console tab (or create a new one and go to https://developer.amazon.com/alexa/console/ask)
  * Click the **Edit** button
  * Select **Endpoint** on the left bar
  * Select the **AWS Lambda ARN** radio button
  * Paste the copied Lambda **ARN** (e.g. `arn:aws:lambda:us-east-1:735534501522:function:alexaSkillTeam777`) in the **Default Region** box
  * Click the **Save Endpoints** button
  * Click the **Test** link on the top menu bar
  * Enable testing by turning on the switch
  * Type **space facts** in the **Alexa Simulator** text box and press the [ENTER] key
  * You should hear Alexa reading an space fact that comes from the Lambda function
  
*Congratulations!!!* You just created your first fully functional Alexa skill!

# So, my Alexa skill works... but how?

Follow the below steps, and links to the documentation, to understand more about how an Alexa skill is put together.

* The above steps helped you to associate the Alexa skill with an [**AWS Lambda Function**](https://aws.amazon.com/lambda/).  
  * AWS Lambda is essentially a service where you can run some code, take in some input, and produce output without having to think about deploying the code or having any kind of server environment setup.
  * Our Lambda is being triggered, or invoked, by the **Alexa Skills Kit** whenever you test the input of your Alexa skill.

* Let's take a closer look at the [facts skill interaction model](https://raw.githubusercontent.com/curtiseinsmann/alexa-hackathon/master/facts-skill-interaction-model.json), which gets built into the **Alexa Skills Kit**.
  * First, there's the **Invocation name**, *space facts*.  This is generally the catch phrase that lets Alexa know to invoke your skill (which invokes the Lambda function you connected to it).
  * Next, there are the [**intents**](https://developer.amazon.com/docs/custom-skills/create-the-interaction-model-for-your-skill.html#intents-and-slots).  A skill can have multiple intents.  Look at the intents in the interaction model, then look through the `index.js` code in your AWS Lambda function to see where they are being invoked.
  * The `GetNewFactIntent` is the one that actually gives you some space facts, in this case.  This intent, in particular, has [**sample utterances**](https://developer.amazon.com/docs/custom-skills/create-the-interaction-model-for-your-skill.html#sample-utterances) used to invoke the intent.  The other intents are [provided by the Alexa SDK](https://developer.amazon.com/docs/custom-skills/implement-the-built-in-intents.html#implement-built-in).
  * [**Slots**](https://developer.amazon.com/docs/custom-skills/slot-type-reference.html) are not used by this particular interaction model, but they're useful for getting user input to your function, e.g. numbers, dates, etc.

* It will be beneficial for your understanding if you try a few tweaks to the `index.js` code from your AWS Lambda function, as well as to the *interaction model* in the Alexa Skills Kit.  
  * Can you add a new space fact?
  * Can you add a new intent to the Alexa skill? (This will require that you rebuild the model in the Alexa Skills Kit.)
  * Can you add some customized slots, and modify the output based on the input?

* Now would be a good time to [set up your Echo Dot](https://www.amazon.com/gp/help/customer/display.html/ref=aw?ie=UTF8&nodeId=201994280) so that you can verify everything is working smoothly with voice input/output.

# Create Your Own Alexa Skill

Now, work together with your team to come up with your own Alexa skill.  Be creative, and have fun with this!  Here are some tips to get you on the right track:

* Alexa skills range from from simple input/output models, to complex and interactive models.  You can find some examples and ideas at https://github.com/alexa
  * Think about time scope when choosing an idea -- we only have a few hours to work on this!  Best to choose an idea that you know you can get working, then build off of that if you have additional time.
* When creating a new function in the *Lambda* console, filtering the Blueprints on **alexa** will give you other examples of some more complicated skills.  When you come up with an idea, try to choose a blueprint that closely matches the desired structure of the skill you're trying to create.
* It's probably best to just stick with `nodejs` or `python` templates -- this will allow you to get off the ground quickly.  Java is supported by AWS Lambda, but it may be difficult to get a Java program running in the limited time that we have today.
* You'll want to reference [the Alexa Skills Kit documentation](https://developer.amazon.com/docs/ask-overviews/build-skills-with-the-alexa-skills-kit.html) throughout this hackathon.

# Other Tips

* For faster testing, you can test your Lambda function directly through the AWS Console, if you want.
  * Copy the json content of the **JSON Input** panel in the Alexa Console
  * Go back the Lambda function tab
  * Select the **Configure test events** option from the dropdown to the left of the **Test** button (upper right section)
  * Paste the json content into the editor
  * Type a **Name** and then click the **Save** button
  * Click the **Test** button (upper right section)
  * Review the **Details** of the Execution results
