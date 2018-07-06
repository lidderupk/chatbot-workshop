# Watson Assistant Workshop Lab

<!-- TOC depthFrom:2 updateOnSave:false -->

- [Introduction to Watson Assistant](#introduction-to-watson-assistant)
- [Step 1: Sign up on IBM Cloud](#step-1-sign-up-on-ibm-cloud)
- [Step 2: Train Watson Assistant Service](#step-2-train-watson-assistant-service)
    - [Intents, Entities and Dialog](#intents-entities-and-dialog)
    - [Slots](#slots)
    - [Handlers](#handlers)
    - [Digressions](#digressions)
- [Step 3: Test Watson Assistant Service](#step-3-test-watson-assistant-service)
    - [Basic Dialog and Slots](#basic-dialog-and-slots)
    - [Digression](#digression)
    - [Handler](#handler)
- [Step 4: Deploy to Slack](#step-4-deploy-to-slack)
    - [Alternatively, Deploy to Node.js Application](#alternatively-deploy-to-nodejs-application)
- [Final Demo](#final-demo)
- [Summary](#summary)
- [License](#license)

<!-- /TOC -->

## Introduction to Watson Assistant

The Watson Assistant service combines machine learning, natural language understanding, and integrated dialog tools to create conversation flows between your apps and your users. In this lab, you will create a workspace and understand the terminology of creating a chatbot.

## Step 1: Sign up on IBM Cloud

Sign up on [IBM Cloud](http://ibm.biz/slackchatbot070918). If you already have an account, sign into your account.

## Step 2: Train Watson Assistant Service

### Intents, Entities and Dialog

1.  Click on the **Catalog** link in the top-right corner of the IBM Cloud dashboard.

2.  Select the **Watson Assistant** tile under the section titled Watson.

    ![IBM Cloud catalog](assets/catalog.png)

3.  You can choose a custom name for the service, or leave it as the default value. Select the region, organization, and space desired. Click on **Create**.

    ![Enter Service Name](assets/servicename.png)

4.  Click on the **Launch tool** button to launch into the Watson Assistant tool.

    ![Launch Tool](assets/launchtool.png)

5.  This is the Watson Assistant tool where you can create workspaces and setup different chatbots dialogues and applications. Select the tab labeled **Workspaces**.

    ![Watson Assistant Start Page](assets/startpage.png)

6.  There is an example Customer Service workspace where you can see a more evolved training. However, we'll create a new workspace for our bot to use. Click on the **Create** button in the box labeled **Create a new workspace**.

    ![Workspaces](assets/workspaces.png)

7.  Enter a name for the chatbot and a description. Click **Create** when finished.

    ![Create workspace](assets/createworkspace.png)

8.  You will be redirected into a page with three tabs: Intents, Entities and Dialog. Under the Intents tab, click **Add intent** to create the first intent.

    ![Add New Intent](assets/addnew.png)

9.  Name the intent `book_reservation` and use the answers you wrote in Step 1 to create the first intent. If you did not go through that exercise, enter the following examples for the intent:

    ```
    book a reservation
    book a table
    make a reservation
    reserve a table
    schedule a reservation
    secure a reservation
    ```

    ![Create intent](assets/intents.png)

10. Click the back arrow to go back to the **Build** page. Click on the **Entities** tab in the top menu bar. This is where you can add entities. Add the entity you wrote in Step 1. If you are not following that exercise, add the `cuisine` entity with the following values

    ```
    american
    chinese
    italian
    mediterranean
    mexican
    ```

    ![Create entity](assets/entities.png)

11. The Watson Assistant service has a handful of common entities created by IBM that can be used across any use case. These entities include: date, time, currency, percentage, and numbers. Click on **System entities** sub tab. Toggle on the switch for `@sys-time`, `@sys-date`, and `@sys-number` to enable the entities.

    ![Enable System Entities](assets/systementities.png)

12. Click on the **Dialog** tab in the top menu bar. Click **Create**. There are two nodes added by default. The `welcome` condition is triggered when the chatbot is initially started. This is a good place to introduce the bot and suggest actions the user can ask of this chatbot. Enter `Hi, I'm HungerBot. You can ask to reserve a table.` Click the `cross` icon to leave this screen and go back to the `dialog` page.

    ![Create dialog](assets/dialog.png)

13. The second node checks for the condition `anything_else`. In the event the user enters something that wasn't expected, the service will return this response. Ideally, it should convey a way for the user to recover, such as example phrases.

    ![Create anything_else node](assets/anything_else.png)

    **Try it**

14. Return back to the `welcome` node and click on the three dots on the right side of the node. Select **Add node below** from the menu.

    ![Add node below](assets/addnode.png)

### Slots

15. Name this node `Book Reservation`. Add a node to test the condition of the first intent you created, `#book_reservation`, as shown below.

    ![Book Reservation](assets/bookreservation.png)

16. Click on **Customize** in the top right corner. Toggle the slider labeled **Slots** and tick the box labeled **Prompt for everything**. Click **Apply**. Slots will help to gather multiple pieces of information needed in order to book a reservation.

    ![Enable Slots](assets/enableslots.png)

17. Add a slot for `@cuisine`, with the prompt:

    `What type of cuisine would you like?`

    ![Add cuisine slot](assets/addslots1.png)

18. Add another slot for `@sys-date`, with the prompt:

    `What day would you like to reserve?`

    ![Add date slot](assets/addslots2.png)

19. Add another slot for `@sys-time`, with the prompt:

    `What time would you like to reserve?`

    ![Add date slot](assets/addslots3.png)

20. Add another slot for `@sys-number`, with the prompt:

    `How many people will be coming?`

    ![Add size slot](assets/addslots4.png)

21. If no slots are pre-filled, prompt the user to provide a cuisine:

    `Sure, I can help make a reservation. What type of cuisine did you want?`

    ![Add initial slot](assets/addslots5.png)

22. Have the bot respond with the details of the reservation:

    ```
    Great! I've booked a table for <? $number ?> people on <? $date ?> at <? $time ?> for <? $cuisine ?>.
    ```

    The <? … ?> syntax uses the values stored in the context and injects the values into the response.

    ![Respond with confirmation](assets/respond.png)

    **Try it**
### Handlers

We can use handlers to give an exit route to the user.

23. Go back to the **Build** page by clicking on the cross on top of the page. Add a #Cancel intent.

    Use the following as user examples
    ```
    cancel that
    cancel the request
    forget it man !
    I changed my mind
    I don't want a table anymore
    I have other plans.
    nevermind
    never mind
    ```

    ![Respond  with confirmation](assets/cancel-intent.jpg)

24. Use the **back button** to go back to the **dialog** page. Open the "book a reservation" node and click on "Manage Handlers"

25. Enter #Cancel in the `If bot recognizes` to indicate this handler should be invoked when the user wants to cancel the reservation in the middle of booking it.

    `Okay, cancelling your request.`

    ![Respond  with confirmation](assets/cancel-handler-1.jpg)

    Click on the gear icon to open the detail pane. Since the user is cancelling their reservation, we need to clear out the context variables for this conversation. Additionally, we are setting the `$user_cancelled` variable for the next node to show the appropriate response.

26. On the detail page, click on the three dots and then `Open context editor`. Set each one of our context variables (defined earlier as slots) to null.

    ```
        cuisine
        date
        time
        number
    ```

27. Additionally, add another context variable `user_cancelled` and set it to `true`. We will use this in the next step to respond accordingly for the whole node. Finally, set `And finally` to `skip to response`. The user has decided to abandon the intent, so we don't want to ask any more questions and just skip to the response.

    ![Respond  with confirmation](assets/cancel-handler-2.jpg)

28. Click `back` to save the state and go back to the handler page. Click `save` to exit this back go back to the `Book Reservation` node. 

29. Back in the main node, click on the `Customize` button on top right to open the detail pane. Enable `Multiple responses` so that we can check for the `user_cancelled` condition and response accordingly. Click on `Apply` to close the pane.

30. Add a new response and then move it up before the existing response by using the up arrow. **The order is important here** as the flow will stop on the first condition that returns true. Use `$user_cancelled` in the first conditional response with the response of `Sorry to see you go. You are always welcome to start again.`

31. Click on the gear to go into the respond. We want to stop processing the rest of the response and simply go back to the welcome node. This way the user can start again. Select `Jump to ...` in the `finally` case and select `Welcome` node to jump to. You are presented with multiple options to follow after the jump. Select `Wait for user input`.

32. Additionally, set the $user_cancelled variable to false here. Click `Save` to go back to `Book Reservation` node. For the second response, add the condition of `true`. This is the catch all and will only be reached if they have not cancelled.

**Try it**

### Digressions

33. Go back to the `Build` page. Let's add a way for the user to ask common questions like `What are your hours of operation ?` and `What kind of cuisines do you offer ?`. We will first have to recognize what the user is asking for. Add an intent called `faq_cuisine_type` and give it the following examples:

    ```
    Show me all cuisines
    What are my cuisine options ?
    What can I eat here ?
    What kind of cuisines do you offer ?
    What's on the menu ?
    ```

    ![Respond  with confirmation](assets/cuisine-intent-2.jpg)

34. Click on the back arrow to go back to the `Build` page. Add another intent for `faq_hours` with the following examples:

    ```
    Are you open on the weekend ?
    Do you work on the weekends ?
    How late are you open ?
    What are you hours ?
    What days are you open ?
    What time do you open ?
    What time do you close ?
    ```

    ![Respond  with confirmation](assets/faq-hours-intent.jpg)

Now let's add a dialog for FAQ. Click on the back arrow to go back to the `Build` page.

35. Click on the three dots on the `Book reservation` node and select `Add a folder`. Name this folder `FAQ`

    ![Respond  with confirmation](assets/digress-add-faq.jpg)

    ![Respond  with confirmation](assets/digress-name-faq.jpg)

36. Click on `Customize` button and enable `Allow digressions into this folder` and also `Return after digression`. Click `Apply` to save your changes.

    ![Respond  with confirmation](assets/digress-customize-faq.jpg)

37. Click on cross to go back to the `Build` page. Click on the three dots in the `FAQ` folder and select `Add node to folder`. 
    ![Respond  with confirmation](assets/diregress-add-node.jpg)

38. Name this node `FAQ Cuisine` and add the `#faq_cuisine_type` intent you just created in the `If bot recognizes`. If the user is asking about cuisine, add the following responses:
    ```
    We offer delicious Mexican, Chinese, Italian, Mediterranean,  and American cuisines.
    Join us for Mexican, Italian, Mediterranean, American or Chinese.
    What about some spicy Mexican, awesome Mediterranean, good old American, exotic Chinese or some Italian pasta.
    ```

    ![Respond  with confirmation](assets/digress-faq-cuisine-node.jpg)
    
39. Click on the cross button to go back to the `Build` page. Click on the three dots in the `FAQ` folder and select `Add node to folder`. 

    ![Respond  with confirmation](assets/faq-add-hours-folder.jpg)

40. Name this node `FAQ Hours` and add the `#faq_hours` intent in the `If bot recognizes`. If the is asking for hours or when the restaurant is open, respond with the following:

    ```
    We are open from 9am to 9pm daily.
    ```

    ![Respond  with confirmation](assets/digress-faq-hours-node.jpg)

41. Click cross to go back to the `Build` page. Open the `Book reservation` node and click on `Customize`. Go to the `Digressions` tab and enable `Allow digressions away while slot filling`. Also enable `Only digress from slots to nodes that allow returns`. Click `Apply` to save your changes. Click cross to go back to the `Build` page.

    ![Respond  with confirmation](assets/digress-customize-book-reservation.jpg)

**Try it**

## Step 3: Test Watson Assistant Service

### Basic Dialog and Slots

The Watson Assistant tool offers a testing panel to test phrases to confirm the correct intents, entities, and dialog are matched and returned.

1.  To test the bot, click on the **Try It** button in the top-right corner of the tool.

    ![Ask Watson](assets/askwatson.png)

2.  A side panel appears and shows the response of the node that matches welcome. Enter a message that triggers the `#book_reservation` intent. We can ask `book a table`

    ![Book Reseration](assets/test-intro.png)

3.  Notice that the intent `#book_reservation` was recognized. The `#book_reservation` node was triggered and the output includes the response from the Book Reservation node. The user is prompted for a choice of cuisine.

    ![Book Reseration](assets/test-bookreservation.png)

4.  When the user enters a cuisine, the `@cuisine` entity is recognized with a value `american`.

    ![Enter type of cuisine](assets/test-cuisine.png)

5.  When the user enters a date or time, Watson extracts out the value using the system entities `@sys-date` and `@sys-time`.

    ![Enter date and time](assets/test-date.png)

6.  Finally, when the user enters a number (either numerically or spelled out) for the number of people in the reservation, Watson extracts out the number using the system entity `@sys-number`.

    When all the required information (slots) is provided, the chatbot responds with a summary of the reservation. The provided valudes are injected into the response.

    ![Enter number in party](assets/test-size.png)

### Digression

0.  Click the clear button to clear the screen and clean up the context variables.

1.  Enter `Book a reservation`. The bot again identifies the correct intent and starts by trying to fill out the first `cuisine` slot.

    ![Enter number in party](assets/test-digress-1.jpg)

1.  Enter `What are my options ?`. The bot identifies this as a digression to the cuisine node based on the #faq_cuisine_type intent. It then asks for the cuisine information again.

    ![Enter number in party](assets/test-digress-2.jpg)

1.  Enter `I would like to have Mexican at 5pm tomorrow. Please book a table for 4`. The bot finishes the task at this point.

    ![Enter number in party](assets/test-digress-3.jpg)

### Handler

0.  Click the clear button to clear the screen and clean up the context variables.

1.  Enter `Book a reservation`. The bot again identifies the correct intent and starts by trying to fill out the first `cuisine` slot.

    ![Enter number in party](assets/test-digress-1.jpg)

1.  Type `Nevermind, I am not hungry anymore`. The bot recognizes the `Cancel` handler on the `#book_reservation` intent and cancels the whole request.

    ![Enter number in party](assets/test-handler-1.jpg)

1.  It clears out the context variables as well.

    ![Enter number in party](assets/test-handler-2.jpg)

## Step 4: Deploy to Slack

1.  In the Watson Assistant tool, click on the **Deploy** icon in the left sidebar menu of the Watson Conversation tooling.

    ![Deploy Watson Conversation](assets/deploy.png)

2.  Click on **Deploy** under the tile labeled Slack.

    ![Deploy Watson Conversation](assets/deployslack.png)

3.  Follow the instructions to create a Slack application.

    ![Deploy to Slack](assets/slack.png)

### Alternatively, Deploy to Node.js Application

A [sample Node.js application](https://github.com/watson-developer-cloud/assistant-simple) that uses a webbased chat interface is available in the Watson Developer Cloud on Github.

1.  Clone the repo:

    ```
    git clone https://github.com/watson-developer-cloud/assistant-simple
    ```

2.  Follow the instructions to either run the application locally or in IBM Cloud. Add the Watson Assistant service credentials (username, password, and workspace ID) either in the .env file (locally), or bind the Watson Service to the IBM Cloud application.

    ![Watson Assistant Credentials](assets/credentials.png)

    ```
    # .env file
    # Environment variables
    WORKSPACE_ID=<workspace-id>
    ASSISTANT_USERNAME=<conversation-username>
    ASSISTANT_PASSWORD=<conversation-password>
    ```

3.  Run the application:

    ```
    npm start
    ```

4.  Open a browser to the application and begin interacting with the chatbot.

        ![Watson Assistant Sample Node.js application](assets/assistant-simple.png)

    hello world

## Final Demo

[![Alt text](https://img.youtube.com/vi/8c1JYL3aFRs/0.jpg)](https://www.youtube.com/watch?v=8c1JYL3aFRs)

## Summary

The Watson Assistant service was able to handle gathering multiple pieces of information, parsing the user input, and placing the values into a context that was used to inject into the response back to the user.

## License

© Copyright IBM Corporation 2018

IBM, the IBM logo and ibm.com are trademarks of International Business Machines Corp., registered in many jurisdictions worldwide. Other product and service names might be trademarks of IBM or other companies. A current list of IBM trademarks is available on the Web at &quot;Copyright and trademark information&quot; at www.ibm.com/legal/copytrade.shtml.

This document is current as of the initial date of publication and may be changed by IBM at any time.

The information contained in these materials is provided for informational purposes only, and is provided AS IS without warranty of any kind, express or implied. IBM shall not be responsible for any damages arising out of the use of, or otherwise related to, these materials. Nothing contained in these materials is intended to, nor shall have the effect of, creating any warranties or representations from IBM or its suppliers or licensors, or altering the terms and conditions of the applicable license agreement governing the use of IBM software. References in these materials to IBM products, programs, or services do not imply that they will be available in all countries in which IBM operates. This information is based on current IBM product plans and strategy, which are subject to change by IBM without notice. Product release dates and/or capabilities referenced in these materials may change at any time at IBM&#39;s sole discretion based on market opportunities or other factors, and are not intended to be a commitment to future product or feature availability in any way.

---
