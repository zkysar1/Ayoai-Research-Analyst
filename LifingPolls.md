# 📋 LifingPolls

- [ ] **Filtering Algorithm(s) Build**

    - [ ] **Be able to archieve a poll.  But how will the stats no something was archieved?  And how long it was archived?  Maybe it needs another table?  Called archived times.**
    - [ ] **Ahh man, I need to be able to filter by topic, like "excersise. "   then I can berries the topic, with stats on it, and ranked on what to do.  Only have to sort on what catagory it is.  Have one sorter that accepts parameters, like filters, like poll ID, and response ID, and then something else, like cstagorie.  Build a make cacagory page, and ask what, make like todoist projects?**
    - [ ] **Need to add archive flag and bitmap to Java but not sql.  And I'll have to do this more often, so document it..  No will jsut set the frequency to zero.  Have note that says make zero for non frequency polls.**
    - [ ] **If shared preferences not there (the sort type) then use default.  Have list view of sort options.**
    - [ ] **Do a slider on the fragment view. I'd want all, frequent, non frequent, archieved. Or just have buttons, and the resets with different sort types.**
    - [ ] **Save the sort type in sql, actually.   Its filter type.   That way we can use that to partion how much data we load.  This means I now need a user table.  Can I store phone name for now?**
    - [ ] **I think I got it.  Call it !!Poll Filter!! drop down (dailys, non-dailies, weeklies, monthlies yearlies).  Call it !!User Filter!! drop down (Me, Friends, Public).  !!Sort!! drop down (start date)  alphabetical, etc).  !!Response Filter!! drop down.  Even then a category drop down.**
    - [ ] **We can worry about the saving later.**
    - [ ] **Can I add a text search inside the poll filter page?**
    - [ ] **I will need to get this done by using position numbers, but can I maintain a cuple at once so I do not use a lot of java resourses?**
    - [ ] **Be able to add zero for no date, or 365, no zero, and then filter off them later.**

- [ ] **Adding Users/Sharing**
  
  <details>
  <summary>💬 Comments (3)</summary>
  
  **Comment 1:** Will need the ability for a user to join a poll from a friend, and they know eachother both have the same poll.  

  
  **Comment 2:** Public includes everything, and would need a new data upload, but how much, or sounds like a lot
  
  **Comment 3:** Have poll say public friends or me, but at response level, always give them the chance to stray from the parent.
  
  </details>

    - [ ] **Add comments to responses.**
    - [ ] **When sharing, need to have polls diffent colors, etc.**
    - [ ] **Need to add the sharing feature to the plan.  And maybe decide when encrypting is important.**
    - [ ] **Username and PW**
    - [ ] **For sharing, have friends be able to share goals.**

- [ ] **Encryption/Decryption**

    - [ ] **Add encryption to the plan.  When is the right time to do that.**

- [ ] **Add notes onto polls (a new data table called notes and attached to polls just like responses are).**

    - [ ] **If things hadn't been done in a while, ask to explain gaps?  You were inactive for a few days, what have you been up to? Have ibactivity report poll with auto resoonses.**
    - [ ] **Add new data element called reason why.  Why do I have these goals.  For each poll, so when I postpone it, I know why.**
    
    <details>
    <summary>💬 Comments (1)</summary>
    
    **Comment 1:** The "reasons why" are the goals
    
    </details>
    - [ ] **Reason why.  My back hurts after. 3 days.  Does it though, I need a back hurt poll.  Link polls.  Say these polls, like go running, helps my back pain poll? poll helps me**
    - [ ] **Why did you break your streak? Have it auto pop up for each poll.  Have PollReasons.   Then use cab enter them, and then check a boolean that asks if you want to enter a reason each time break a streak?**
    
    <details>
    <summary>💬 Comments (5)</summary>
    
    **Comment 1:** Only ask what I need
    
    **Comment 2:** My tummy feels weak when I don't do it.  Like bloded. 
    
    **Comment 3:** Find that story, the writer.  Say I want build apps like this.  That "know what I need and only show me what I need.". The filter I need to have the timlies. We have to be smart about it. Have smart algorythems. 
    
    **Comment 4:** Have the boolean ask afterter how many streak breaks to ask? Trigger this question as the poll loads. 
    
    **Comment 5:** Reasons for sit-ups.  Your going to be doing them the rest of your life.  There is a pain on right side muscle tissue.  It prevents fat cells from consentraiting  thier due to the increased activity in the area. 
    
    </details>
    - [ ] **For this, make it more note like.  And have catgory.  One catagory should be "Why answer this poll"  Another one could "What's in your way"**
    - [ ] **In addition to responding to the polls, the user can also write notes down about the poll, such as improvements, sustains, reasons why, and what may be in your way.**
    - [ ] **And we would still want poll notes kept off to the side.  in a serpeate table,.  I would likly only load these via sql at time of requesting, and not inserting into main model, because analytics is not needed on them.**

- [ ] **Activity board (new data table called activities, saves activity and therefore some history on changes.  Attached to the user.)**

    - [ ] **Have an activity view with all the responses sorted by time, not by poll.   This will cause a hige resort every time I swtich between the two.**
    - [ ] **Have top level swipe left and right for Poll list and ResponseActivity list.  Then, when viewing poll, have another swipe to all responses, then swipe into another fragment for analytics.  Eventually, hop into another one that has recent comments.**
    - [ ] **I need to make an activity board, I need to save history.  Like, have an activity table.  In that table I'd have to store likes, and what changed, etc.  Right?  That way I can make a cool activity board.  And have history, and be able to revert history.**
    - [ ] **Oh, can I also add on a task.  And assign it to dates.  Could be exact same table structures for both.  Maybe have two instances.  Use poll for a project.  If no date for the response (which is the task) provided, use null to only show at project level.  Then there could be an activity stage.  Maybe not though, who knows.  Likly not.**
    
    <details>
    <summary>💬 Comments (1)</summary>
    
    **Comment 1:** No would not work.  Because it's a completly diffent structure lol.  Like tasks would have to show up and you check them off
    
    </details>
    - [ ] **Another reason why I need this, is that I add a poll on a past date, but no one will get the update if its from the past.  So there has to be a psoting date, and another date.  I already have that.  And hey, instead of a response data table, and an activities datatable, just have all in same, then have a category.  Like responses, then edited, changed text, changed date, added poll, added response.  I can then say put any added responses intop the feed, and do or do not put other reponses in the feed. I say we do.  And it coudl say, Zka added this to their past: and do not show it later on the activity borad.  Of course show it later in the their responses.  even on poll, I would have a response boeard, and then an activity board, but they can all come from same data set.**
    
    <details>
    <summary>💬 Comments (2)</summary>
    
    **Comment 1:** And we would still want poll notes kept off to the side.  in a serpeate table,.  I would likly only load these via sql at time of requesting, and not inserting into main model, because analytics is not needed on them.  
    
    **Comment 2:** Then I can have a growing hash.  When inputting the data, on first scrum, I can count the total sum of each category, no rthat would not work because we  would need to keep this sorted.  
    
    </details>

- [ ] **Front end UI improvements or reports**

    - [ ] **Also need to add in a break inside recycler view for a new day.  Or make the Day stand out better.  Like Thursday, instead of saying who date string.**
    - [ ] **Change again so once response is submited go back to the poll, instead of polls list.  !!Or add two response buttons!!, one that says respond and skip to poll list**
    - [ ] **Need to make scroll view on response so that the preview dies not get distorted.**
    - [ ] **How to make a click and zoom in close picture.**
    - [ ] **Add a privacy top right sandwich bar.  Have it have a privacy tab, that explains our trust.  Ranger Safe.  Hi, I'm Zachary Kysar, ex army ranger father of two, married and go camping in new eng land I like my privacy.  We encrypt with this.  Everything stays in house, as ll login.  Securely backed up on arvixe.  Even if the data is breached there, they'll find it all encrypted.  Even Someone who has access to maintain the server, only sees encryption.  Give an example.**
    
    <details>
    <summary>💬 Comments (5)</summary>
    
    **Comment 1:** 
    
    **Comment 2:** 
    
    **Comment 3:** Memory management and data model.  Is we load onto memory and and dump after the session. 
    
    **Comment 4:** It's stored on phone and in the cloud.  The file names are hashed, and no connection to the owner.  Hosted on arvixe.   
    
    **Comment 5:** Sharing is strictly enforced, and leans on the side of exclusive.  
    
    </details>
    - [ ] **I could add a refresh button that that goes to main activity so it triggers full refresh of data.  Then add an alert on the other end when a sql connection failed.  It works be petty resiliant that way.**

- [ ] **Searching Algorithm(s) Build**

    - [ ] **Show me counts of months, into past, that would be easy to display.  Then have list of arrays with Mac count calculated. Have a series of zeros in the array. Get Markie to help.**
    
    <details>
    <summary>💬 Comments (3)</summary>
    
    **Comment 1:** Would need to add what day of week they occur.  Have the weekly review.
    
    **Comment 2:** Both with the same searches.  Drop down to select polls.  Drop down to  select hash tags
    
    **Comment 3:** Or can reclutter the table with any filter.  Can I actually use a filtering service, to pass in the filtered values and return.  Anyway.  The same search code should be applied to the table and the today mode.  Reflect mode.  Can alter the dates. CN both have daily mode and reflection mode.both with the
    
    </details>
    - [ ] **Then can have search, that searches all the data in those iterations. Then reclutters the chart based on those searches.  And can click on chart to see activity.**
    - [ ] **Phrases to normalize at poll level.  "both days this weekend" skills be normalized too "on Saturday or sunday".  Even, on both mornings this weekend".  Times as well.  That's we we ships be able to applie these normalized values when searching.  And then can encode and decide.**
    - [ ] **If I make a binary tree of words, for searches conected back to their polls, run each word by another table to combine them.  Then when I search, and hit a word in the table, I know it means what I'm looking for.  Yikes f that lol.**
    
    <details>
    <summary>💬 Comments (2)</summary>
    
    **Comment 1:** No, just a sit contains search on the Blob in after, right?  No, not for every thing. 
    
    **Comment 2:** But I skulls do a binary tree for all words for searching.  And likly get rid of common ones.  
    
    </details>
    - [ ] **Need to be able to search keywords.  Likly jsut a contains search?  Although, I want polls to pop up as I type more than one letter, etc.  And then have it sorted based on importance.**
    - [ ] **Drawing of front end idea to house the new buttons.**
    
    <details>
    <summary>💬 Comments (1)</summary>
    
    **Comment 1:** 
    
    </details>
    - [ ] **When I search, I should basically be applying positions to each node if they contain something. This is way less comparisons int he end than doing it with sql.**

- [ ] **Data modeling tweaks**

    - [ ] **Need to make the database refresh actually work with last updated times and stuff.**
    - [ ] **Add check that says, if no connection, alert user, say not added.**
    
    <details>
    <summary>💬 Comments (1)</summary>
    
    **Comment 1:** Need to make a toast message when the server is not succesful.  Or somhow prevent saying it is possible to add when no internet.  or both.
    
    </details>
    - [ ] **Figure out what logs do what.  I woudln;t want to be printing a shit ton of logs on my phone all the time.  Need to clean up when I am printing the lists.  and all the logs I dont' need anymore.  Log d is for decoding, log i is for informational purposes.**
    - [ ] **Make a combine function, to combine two polls, and delete one.  Or all witch one to keep as the main.  Oh, and I really need to do the "combine" thing.  Combine a poll.  Like what did I do wrong at work and what did I do right at work.  Shills be the same.**
    
    <details>
    <summary>💬 Comments (1)</summary>
    
    **Comment 1:** Add feature to combine polls, and auto combine their responses.  (I already have a need to consolidated some detailed polls into more generalized ones.)(When is good time to do this?)
    
    </details>
    - [ ] **Start downloading data better, smarter, faster, less battery.  In class notes from one guy.**
    
    <details>
    <summary>💬 Comments (7)</summary>
    
    **Comment 1:** Is it problematic to have all data stored in main memory?  In my two lists?  Or should I get smart and not save much there?
    
    **Comment 2:** In class this guy used a class that extends "worker," that will update things every so often during the day.  And be Danes to SQLlite first, and only uploads to database is a connection.  If not, someway to identify it in sql light actually.
    
    **Comment 3:** To load the data faster, I could keep an int on Sql to represent what to load first.  So I could have aqury that says load everything in group a, then everything on group b, etc, which would have been set from the previous sort!
    
    **Comment 4:** While the position count increases on each recycler view, I could maybe load the next 100 public id's up.  Wouldn't want this for personal because I want more stats.
    
    **Comment 5:** Need to start hashing the responses
    
    **Comment 6:** And to make faster, I can store data on sql lite?  Now, because I need in Java for the stats?  Maybe not?
    
    **Comment 7:** When I refresh stats, I can get count, and.
    
    </details>
    - [ ] **Polling ideas tweaks.  Like, be able to answer multiple polls with jsut one response.  Like, if someone is sick, I woudl want to add that to the sickness poll and to the emma kid poll.  If if thre is a storm, you could add it to two storm polls, one for weather and one for snow accumulation.   Like what is the differnce between a poll and a catagory.  This has implications that one response can be with mutliple polls, what does that do to data structures?**
    - [ ] **Pivot from loading everything into main memory to using sql back and forth**
    - [ ] **How will sharing affect this model?  How will we populate external data onto screen, from one-by-one sql statements?**
    - [ ] **Then sycn data that is saved on the phone.  But save it encrypted everywhere.**

- [ ] **Additional options for poll responses**

    - [ ] **Need to implement answer type.  Have yes/no and int only and free text, then have different validitions.**
    - [ ] **Auto add responses to polls.  Like, if open this app, make response.**
    - [ ] **Another aspect is Poll answer types. Eventually the app would want to support yes/no answers, or categorical choices. This would aid in analytics as well. Another area of growth.**

- [ ] **Ingest third party data**

    - [ ] **You should be able to import google calendars as polls to use for searching.**
    - [ ] **Eventually pull in data from third parties, like Google maps, and calandar**
    
    <details>
    <summary>💬 Comments (1)</summary>
    
    **Comment 1:** (although, we could eventually suck in data not part of the app to enrich it later, like calendar events).
    
    </details>

- [ ] **Add labels to help with filtering and searching**

    - [ ] **Add labels to responses.  Just do the pound sign method for now.**
    - [ ] **Need to add lables for projects, and those same lables for responses.  Then in the poll view, filter to those lables.  In the reponse review, filter to those lables.**
    - [ ] **What does firbase machine  learning have?  Can I use their logins?**
    - [ ] **Need to be able to fix the dates, regardless of last response.  So a boolean that says auto calculate due date.  And reoccurring based on schedule regardless of response date, and then due date based last response.**
    - [ ] **Need to add labels to each of the responses as well.   Need to add a traditions button.  Add machine learning to auto build lables, and then someone can edit them later.**
    - [ ] **Need to be able to make poll due on certain day.**
    - [ ] **Additional options for when polls are due**

- [ ] **Add goal tracking statistics**

    - [ ] **Yeah, add streaks.  No breaks between 7 days.  Yeah!  The charts should be in same iteration, like one, or 7, etc.**
    - [ ] **Need to add average days.  That would be a good test to see if I need to do analytics on board, and when.**
    - [ ] **The poll list view should have stats at the bottom, like: avg days, latest streak.**
    - [ ] **Add goal Tracking related stats, like streaks (did you ever miss a day doing sit ups, you lost your goal of every day)**

- [ ] **Admin**

    - [ ] **Value could be, what do you typically get done this day.  Or, give me daily report.  And assign this importance, what will the story do fit me I wonder?  If multiple late, how do I know which is more important.**
    - [ ] **Add that quotes document to my lifing polls.  Back fill this data.**
    - [ ] **Intersting wording for the app:  Built for close relationshipsOur tools should cherish our relationships as much as we do. Instead of accumulating likes and followers, we are designing intimate and adaptive spaces that will help you get to know your loved ones, stay present with each other, and reflect your relationships back to you in meaningful ways.Designed for quality of lifeResearch has shown the negative impact of social media on mental health; however, we believe technology platforms can be designed thoughtfully to have the opposite effect. We’re working with psychologists, technologists, and mental health experts to ensure that our products cultivate a positive effect on mental health.Presence is the presentWe want to create a two-way physical and emotional window between you and the people who matter most so you can  feel more connected to the people—not the technology.You are not the productMany social media companies rely on advertising for nearly all of their revenue. When advertisers are the customer, their needs will always come before ours. We want to build a customer relationship directly with you and earn your trust and patronage by creating products that improve and help maintain your social health.Your data is yoursYour conversations, relationships, and personal information belong to you and you should have complete control of your data. Since our products will never be ad-driven, we won’t need to sell your personal data for profit.**
    - [ ] **There are two projects in shared family that need to go across. To lifing polls**

- [ ] **So I need to make adc response, and have one drop down for response type.  Like reasons why have this Pol.  Notes on the poll.  Routueen response. Then, on view poll, basically have a way to filter to those different points. From same table.**

- [ ] **Machine learning project Ideas.  Also see semester 6 machine learning project briefs.**
  
  <details>
  <summary>💬 Comments (5)</summary>
  
  **Comment 1:** What about favorite word, most promonint type of moment. 
  
  **Comment 2:** I should I use it to predict what frequenucy is best for each poll?  Maybe call it, ideal frequency or something like that?
  
  **Comment 3:** I started adding "coronavirus" into many responses.  I would expect those to become labels.  I want to do this for machine learning.  Is that possible?
  
  **Comment 4:** Or learn what frequency would be best?
  
  **Comment 5:** Learn categories, so can search on them later.  I would always want them to learn things corona virus.  Or barret.  kids.  
  
  </details>

- [ ] **Add an urgency flag that will change color of the poll when it is due, so things that are due fit a few days won't bother me.**

- [ ] **Also for machine learning thing, do Average frequency VS. Stated freq.  Ah and when finding labels, also use text from poll title to display responses.**

- [ ] **What about a day view.  And if I don't get it done, erase from the day.  So say I have shaving as a poll, it's really over due.  I say, Sunday is prefect day to do it, so I add it to Sunday. But then I don't get it done on Sunday, oh well just remove it from day view and go back to polls.  But what about notes, where would those go, like these?**

- [ ] **Now that I'm saving start date, I can say postpone until a certain date.  That would be a helpful feature**

- [ ] **Have quick add response buttons as widgets**

- [ ] **Poll ideas**
  
  <details>
  <summary>💬 Comments (1)</summary>
  
  **Comment 1:** Need a prompt?
If you're ready to write but not sure where to start, try a prompt. There are hundreds out there on our wide internet, but here are a few to get you started:
·        What is the best thing that happened to you today? The worst?
·        If you could have any meal right now, what would it be?
·        What topic did you think about the most today?
·        Write the first five words you can think of. Then write a short paragraph about each of them. 
·        Is there something you wanted to say today but didn't? Write it out now.
·        Review a movie you saw recently.
·        What's one thing you're looking forward to?
·        Lay out some plans for your Animal Crossing island.
·        What's a small accomplishment that you're proud of?
·        What's one thing you're not looking forward to?
·        Describe the best beverage you've ever had.
·        What's one thing you wish you'd done today?
·        Describe a childhood experience that you remember particularly clearly.
·        Write a letter to your pet.
·        What's the best trip you've taken?
·        What's a good snack you've eaten? Doesn't have to be the best, just a good one.
·        Go to Google and type in a letter. Then write about the first search suggestion.
·        List 10 things you think are good. Can be anything.
Happy journaling! You'll do great. Remember, there is no way to make a mistake.
 

  
  </details>

- [ ] **Any time.  Daily, page filters.**

- [ ] **Never scheduled (these can be items in a project? Then can make them due on certain day?) .  Dailys.  No hard date, but due.  These are for all things on a frequency.   Then there could be some around like, try to get this done on that day, type of thing.**
  
  <details>
  <summary>💬 Comments (2)</summary>
  
  **Comment 1:** 
  
  **Comment 2:** I don't want to be staring at this all day. 
  
  </details>

- [ ] **OMG, why do I want to do this?  https://www.wonderdads.com/**

- [ ] **I really do just need to be able to combine, when delete do not delete all poles, I like the history, and be able to turn off some polls.  Whne combine, be sure to retain the history?**

- [ ] **2.	Or, what about you’re on thanksgiving, and you always take a picture on thanksgiving, but a new thanksgiving comes and you have not taken a picture yet.  How could a model use dat to predict mistakes?  You could input the year and other input items top strengthen a model.**

- [ ] **Hold the phone!!   Roam research, this looks awesome.  It's like each poll us a page in roam. And each page has comments, and can be linked to other pages.  Hmmmm, think about this.  Also look at notion.  Or mem.ai.   Something like that.**

- [ ] **Emma was catching worms.  My journal, I skulls just be able to type that, and save it as one entry.  Then can make rules to create tags.  In above example, tag would be Emma.**

- [ ] **Decision augmentation**

- [ ] **Build an npc brain to fill in lifing polls to simulate a real person.  And the reasons why you do so thing are critical.  So I need a database of reasons.  To drive what I do, and what ever task has most points and written enough reasons, good or bad, then it creates action.**