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



