# ImportantLessonsInAutomation
This repo includes all the important lessons which we should follow or should be aware of for Automation

### Lesson 1

Today I want to point out that many people do not really try to understand the programming basics.

When I ask candidates in an interview, what is encapsulation and why it is used?

I get very weird answers.

1. Some say that they are hiding data and exposing only implementation. But when I ask why we are doing that, they don't have any answers.
2. Some say there is a king in a castle and if we want to access them we need to know someone in the castle. They also told that King here is variable and the someone is getters and setters.

It is okay to have some reasonable real-time examples but here it does not make any sense to the question.

The answer I expect :

1. When we try to operate on public variables, one can set any values to them. For example, firstName can be set with null, a string with special characters or numbers.

Instead, if we have them as private variables, then in the setter method we can do initial business checks before setting the values to the variables.

The same gets applied for getter methods. You have the control to return a default value if some variable is null or not in the desired state.

Basics: Methods bring better control than variables.

Tip: Try to understand the real basics or motto behind everything we do in programming


### Lesson 2

Your ability (1.) to write clear, modular, and maintainable code, (2.) to refactor a messy codebase into a maintainable code is far more critical than your ability (3.) to design a system that is scalable/fault-tolerant.

It's unfortunate that the companies interview for the ability (3.), but what their real need is people with the capabilities (1.) and (2.). Yes (3.) is essential. But (1.) and (2.) are the foundations irrespective of the title that you are hiring!

Many companies were closed or went bankrupt because of the poorly written and unmaintainable codebase!

### Lesson 3

Don't create frameworks!

"I have created this framework" sounds good in the interview and in the LinkedIn post, but it is counterproductive to the company for which you created this framework.

A framework is a living being that requires constant nurturing. But in reality, it is not happening. We switch teams, or we switch companies. The "framework" becomes the bottleneck because the creator's mental model and the theory behind the framework get lost in the transition.

What was once considered a productivity booster had become a maintenance nightmare!

What do we need to do instead?

Create composable libraries! The only inspiration that you need is the Linux operation system!

### Lesson 4

The longer a feature sits in a separate git branch, the costlier it becomes for development and deployment!

Let's assume you have a feature that requires three to six months of development time, and other team members are working on the same codebase to fix bugs and develop additional features.

The feature branch you have created will have to be synced periodically with the master (or main) branch throughout this period. If there are more changes in the master, developers spend more time merging (and resolving merge conflicts).

That's accidental complexity.

The developers are spending their valuable time doing this useless stuff at the cost of developing the feature itself!

What's the solution?

No, it's not microservice (pun intended)

Divide and conquer. Think hard and develop a plan of how to deliver a feature in chunks. Any logical chunk won't take more than one to two weeks to develop and merge. If it takes more than this period, think even harder and divide it further.


### Lesson 5

One thing which I have found really challenging with regards to Microservice is Testing microservice based applications because As a QA you need to esnure that your micorservice works perfectly even when there are changes in another microservice on which your microservice is dependent on and the other microsevice who are dependent on your microservice also works fine. Some of the practices which we have used to maintain Quality of our application are -

1. Large no of unit test to check quality of our feature.
2. Good amount of contract tests ensuring that the contract between all the microservices are verified and changes in one microservice does not hamper the contract, Giving us the
gurrantee that the other dependent microservices are not hampered. This practise has been followed diligently by all the Teams working on different microservices.
3. At our microservice level we have written some API automation cases although even this is verified by most of our unit tests.

4.we did not write UI Automation cases at microservive level as testing UI requires interaction with different microservices, We have few UI Automation cases covering major application flows and
which is used by everyone to Test their microservice as a whole


### Lesson 6

Want to reduce execution time of UI tests by almost 70%?

Recently, I got a chance to review automated UI tests on mobile from a few live projects.

Some of these tests involved multiple users logging into the app and doing certain actions. The complaint was - the tests were flaky (app not getting launched after first user logouts, second user not able to login back due to cache, etc.) and each test taking around 5 minutes to execute.

The test was designed like below:
Login with user 1, perform 5-7 steps and logout.
Login with user 2, perform 5-7 steps, assert and logout.

So you can see the test was performing around 15-20 UI steps. So no wonder why it took 5 minutes to execute.

So I asked below questions to my student:

1. What are you asserting in this test?
Ans: That user2 can view and approve the request created by user1.

2. Ok, does the test intend to assert that user1 is able to create the request?
Ans: No

3. Ok, are you validating login and logout as part of this test?
Ans: No

After this conversation, I got the real issue - It's the design of the test itself.

- The test is using the UI for preparing the test data (the user1 actions).
- The test is using the UI to prepare the app state i.e. login and navigate to the request page.
- The test is doing unnecessary UI action of logout.

How to get rid of this?

- Use API to create the user1 request. API is fast and reliable. This would save 50% of the UI steps.

- I asked the student if their dev had already implemented some kind of Deep Links for other purposes. The answer was - Yes.

So if the dev can create Deep Links for testing purposes as well, user2 can avoid login through the UI. If the Devs are friendly enough and can implement further Deep Links to land the user on a specific page, then user2 wouldn't even need to navigate to that page through the UI (after the login is done). This would prepare the app state.

- Coming to logout, this can be avoided too. Just need to reset the app before every test so that every test finds the app in the same state no matter if the previous test is passed/failed.

Combine all these optimizations, and you would surely reduce execution time by at least 70%. Plus, the test would be less flaky and just doing what it's supposed to do.

Remember this - "Less is more" in UI automation. Design a test such that it performs less UI actions, not more. This is contradictory to what UI automation is supposed to do, but this is how it works.




