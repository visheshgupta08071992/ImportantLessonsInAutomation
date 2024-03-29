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


### Lesson 7

"Sir, I have two API test cases. TC1 makes a POST call and generates an Order ID. TC2 makes a DELETE call. TC2 needs the Order ID from TC1 so that it can be deleted. How do I pass it?"

I often get asked about this scenario from my students. It's a fairly common scenario, but I see many folks struggle with it.

Most of them end up creating an instance variable for the Order ID and share it among test cases. Additionally, some kind of test dependencies or priorities is set so that the tests execute in a specific order (POST -> UPDATE -> DELETE).

Please avoid doing this.

This will work well only when all tests are passing. But when an intermediate test fails, it will have undesired side effects.
❌ A majority of tests will be skipped.
❌ Since using a shared static variable, there will be chances of data leakage.
❌ There will be challenges passing the same Order ID across test classes.

I would instead try to keep my tests independent and have each of them generate their own test data (Order ID).

The guiding principles are simple:
✅A test must not fail or skip an unrelated test.
✅A test should prepare its own test data. No data sharing among tests.
✅Avoid using instance static variables.

Yes, some extra API calls will be made as part of this, but that's ok given the advantages.

How to accomplish this?
I would create reusable methods for POST, DELETE, etc. and use those wherever required in my test cases.

For e.g.

A test for DELETE would look like this:
String Id = postOrder().get("Id");
assertThat(deleteOrder(Id).getStatusCode(), equalsTo(200));

A test for UPDATE would look like this:
String Id = postOrder().get("Id");
assertThat(updateOrder(Id).getStatusCode(), equalsTo(200));

With this, even if the DELETE test fails, UPDATE would not be skipped. It will be executed and any potential issue with UPDATE would also be unearthed. Both tests will run independently and would add to reliability. There is no data sharing, so no chances of data leakage as well. You can even run those tests in parallel to save time if need be.


### Lesson 8

In REST API automation, using POJOs may result in unnecessary code. So is it really a feasible option?

I hear this often. Today I got the same query.

Yes, this can happen, but mostly as a result of poor design.

POJOs represent objects.
Objects are reused across APIs.
So there is a lot of reusability.

It's not like we need to create seperate POJOs for each API.

POJOs are like blocks.
You will create a limited number of blocks only once, and you will be re-using the same blocks to build different objects.

POJOs have other advantages too: readability, compile time type safety and an indirect validation of JSON schema.

Spring Boot developers often use POJOs while developing REST APIs. If test code can reside in the same dev repo, their POJOs can be reused for automation purposes. So a lot of rework can be avoided. This would also increase collaboration between dev and test.

A lot of POJO boilerplate can also be reduced with libraries like Lombok.

Having said all that, POJOs are not the only way to store objects. Other mechanisms can also be used.

In my view,

👉 POJOs are good for not so complex JSONs (This can be subjective too) with good reusability of objects across APIs. POJOs can be clubbed with various libraries to create templates that can be used to drive the test multiple times.

👉 Files are good for static, huge JSON with no or poor reusability of objects.

👉 If a JSON is used only for a particular test, even storing that as a String within the test itself should suffice.


### Lesson 9

One of my students was asked an interesting interview question - "How to validate API pagination?"

I realized this could be something important for folks who are looking out for #qajobs

So I thought to explore it and come up with scenarios that can be validated automatically using REST Assured.

In layman terms, pagination is used when there is a lot of data to send but the client doesn't want all of it in one go. A page is simply a representation of resources.

In my online REST Assured course, I have used Spotify API (one of the best public APIs out there!). But, I didn't automate pagination even though the API implements it.

Spotify API takes two query parameters, limit and offset. Limit is used to set the number of resources a page should return. Offset is used to set the starting point. For e.g. if there are 10 pages and offset is 5, then the server will send data from the 5th page.

Each Spotify API sends a standard paging object in the response. The fields are href, items, limit, next, offset, previous, total (Spotify URLs are in the comments).

Our job is to validate this object and here're the possible tests to automate:
1. When resources are returned, response should be 200
1. "href" should have a link that returns full results
2. Number of items in the response should match the limit sent in the request
3. "next" should have the link to the next page
4. "previous" should have the link to the previous page if the requested page is not the first page
5. If the request page is the first page, then previous should be null
6. If the request page does not exist, then 404 should be returned
7. When second page is requested, "previous" should have the link to first page

And so on.

More or less, any standard pagination implementation will have this paging object.


### Lesson 10

I have seen majority of the Automation QA folks, they just want to automate for the sake of doing it or to impress management or other functional QA folks. 
But they forget one thing, your roots are from testing, you are a tester end of the day. 
Your automation test should find the bugs, so that it can be leveraged at the time of releases and to write a better automation test case, you need to be really good in your product knowledge. What I see these days everyone is learning automation, just to get some good package or offers from some cool product companies.
And then you hear - hey! I don't do functional testing manually. I do only automation and I do magics - This attitude should be changed. 
EOD, you are loosing the importance of testing. If your automation test is not finding the bugs, it's just a piece of garbage code then. 


### Lesson 11

You have to think like a #tester
And act like a #developer
to become a #rockstar test engineer.

This is a very rare skillset and if you work towards this then you can end up being an extremely valuable asset for any company.

There are lot of people who have brilliant testing mind but poor development skills and at the same time we have so many good developers without having a proper testing mindset.

The future of testing is in the hands of people who have solid engineering skiils with rich testing domain experience.


### Lesson 12

Common API Bugs and Test Cases for API Testing

**1. Wrong Error Codes.(Develops made some mistakes and put wrong status code for Different request).**

Following are correct status Code : </br>
Post Request - 201 ,Created (The request succeeded, and a new resource was created as a result.) </br>
Get Request - 200 , Successful (GET: The resource has been fetched and transmitted in the message body.) </br>
Patch/PUT - 200 , Successful(The resource describing the result of the action is transmitted in the message body.) </br>
Delete - 204 , (No Content ) status code if the action has been enacted and no further information is to be supplied.) </br>

**2. Missing Keys**
You have certain keys which are important , like Id should be integer or not null.
But developer missed that key.

**3.Empty Post or Patch (Update) are not Handled properly.**

**4. We can do Black box Testing in API testing, like there is some input field is there 
Age, we can enter empty field,  less than minimum value ,  within the range, more than maximum value.**

**5. Keys Verification:**
If we have JSON ,XML APis we should verify its that all the keys are coming.

**6. Have to test JSON, XML Schema validation.**

**7.Verify the JSON Schema validation , Verify the Field type, Verify the Mandatory fields**

**8.Verify the Response Header & Negative Test cases response** 

**9. Verify that How the Error codes are handled**

**10.Verify the response HTTP status code**

**11. Valid Response Payload**

**12. Chaining Request verification** 

**13.Verification of API with Data parameters**

**14. End to End CRUD flows Test cases** 

**15. Database integrity Test Cases**

**16.File Upload test cases(with different extension file like pdf, txt, exe etc)**

**Client Error responses:**

400 Bad Request - The server cannot or will not process the request due to something that is perceived to be a client error 
(e.g., malformed request syntax, invalid request message framing, or deceptive request routing).

401 Unauthorized -Although the HTTP standard specifies "unauthorized", semantically this response means "unauthenticated".
That is, the client must authenticate itself to get the requested response.

403 Forbidden -
The client does not have access rights to the content; that is, it is unauthorized, so the server is refusing to give the requested resource. 

404 Not Found -
The server cannot find the requested resource. In the browser, this means the URL is not recognized.
In an API, this can also mean that the endpoint is valid but the resource itself does not exist. 

**Server error responses:**

500 Internal Server Error:
The request method is not supported by the server and cannot be handled. 

502 Bad Gateway -
This error response means that the server, while working as a gateway to get a response needed to handle the request, got an invalid response. 

503 Service Unavailable: The server is not ready to handle the request. 

504 Gateway Timeout -This error response is given when the server is acting as a gateway and cannot get a response in time.


### Lesson 13

*How to avoid token getting logged in RestAssured?*

*First let’s understand what is logging used for in Rest Assured?

When you fire any request via rest assured and if that request if failing with non-200 status code, then Logging the Request fired and Response received from the server will help us to debug the Issue further.

The Problem in Logging Everything ->

When we log everything ,the sensitive Tokens also gets logged along with it with actual value. Generally these logs can be pushed to any specific server if we have or we can look at it in the Jenkins console output. Assume we are running a production sanity for all critical cases and you enabled logging for it. The result is the actual production bearer token will get logged in case of Production Sanity which any person who have access can see.

Syntax to log everything in Rest Assured :-

Response response = given().baseURI(“You base url”).headers(“key”,”value”).when().get(“api-path”).then().log().all().extract().response();

How to avoid it?

The below syntax will help you in avoiding the logging of any sensitive Information of your project —
config(config.logConfig(LogConfig.logConfig().blacklistHeader(“HeaderName”)));

Code Sample -

Response resp = given().
baseUri(“https://lnkd.in/gNm-aNgS").
header(“Content-Type”,”application/json”).
header(“Authorization”,”Bearer token-value”).
config(config.logConfig(LogConfig.logConfig().blacklistHeader(“Authorization”))).
log().all().
when().
get(“/api/path”).
then().
assertThat().
statusCode(200)
.extract()
.response();

Sample Output -

Headers: Authorization=[ BLACKLISTED ]
Accept=*/*
Content-Type=application/json

As you can see above, the Authorization header is blacklisted and thus we have achieved avoiding of sensitive Information getting logged


### Lesson 14
Test automation by itself DOES NOT ensure a product is of high quality because quality isn't just the absence of bugs. It is way way beyond that.

Working as expected is NOT high quality. That's bare minimum quality.


### Lesson 15

I interviewed a candidate with 5+ years experience in API Testing. Based on the #work #experience and the #resume shared, these were some of questions that I discussed

1. Why API Testing?
2. Is there any difference between SOAP and REST API testing? If so, how?
3. Key things you test in an API - let's say a GET API to fetch a list of 5 records with id and name
4. GET vs POST
5. POST vs PUT vs PATCH - example for each
6. What do you mean when you say PUT is idempotent? Conditions for idempotency?
7. Do you follow any strategy for sending large payloads in POST.
8. Is it a good idea to have assertions for request headers when testing apis?
9. How do you get a request header in Rest-Assured?
10. If I send a text file as an input in a POST call, what will be the content-type?
11. Key things to test when you API response feeds into a down stream system?
12. URI vs URL - with a simple example.

13. 𝐂𝐚𝐧 𝐲𝐨𝐮 𝐞𝐱𝐩𝐥𝐚𝐢𝐧 𝐦𝐨𝐫𝐞 𝐚𝐛𝐨𝐮𝐭 𝐭𝐡𝐞 𝐉𝐖𝐓 𝐟𝐨𝐫𝐦𝐚𝐭 𝐚𝐧𝐝 𝐡𝐨𝐰 𝐢𝐭 𝐰𝐨𝐫𝐤𝐬 𝐟𝐨𝐫 𝐚𝐮𝐭𝐡𝐞𝐧𝐭𝐢𝐜𝐚𝐭𝐢𝐨𝐧 𝐢𝐧 𝐚𝐧 𝐀𝐏𝐈﹖

Sure! JWT stands for JSON Web Token. It is a compact and self-contained way for securely transmitting information between parties as a JSON object. JWTs can be used for various purposes, such as authentication and authorization in web applications.

JWTs are composed of three parts: a header, a payload, and a signature. Let me explain each part in more detail:

1. Header: The header contains information about how the JWT is encoded and signed. It typically consists of two parts: the algorithm used for signing the token and the token type itself (which is JWT).

Example: 
```
{
  "alg": "HS256",
  "typ": "JWT"
}
```

2. Payload: The payload contains the actual claims or statements about the user or any other data. These claims can be any custom information or predefined standard claims like the user's ID, username, email, role, etc.

Example:
```
{
  "sub": "1234567890",
  "name": "John Doe",
  "email": "johndoe@example.com",
  "role": "admin"
}
```

3. Signature: The signature is a combination of the header, payload, and a secret key. It is used to verify the integrity of the token and ensure that it has not been tampered with. The server can use this signature to authenticate the token and determine if it is valid.

Example:
```
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secretKey
)
```

To summarize the process, here are the steps involved in working with JWTs:

1. Authentication: When a user logs in or authenticates, the server generates a JWT and sends it back to the client.

2. Authorization: The client includes the JWT in subsequent requests to the server, typically in the Authorization header using the Bearer scheme (e.g., "Bearer <token>"). This provides proof of the user's identity and allows the server to authorize the requested actions based on the claims in the payload.

3. Token Verification: On the server side, the received JWT is verified by validating its signature using the secret key. If the signature is valid, the server can trust the information in the JWT and proceed with processing the request.

4. Refreshing and Expiration: JWTs can have an expiration date/time set in the payload. If a token expires, the client needs to obtain a new one by re-authenticating.

Overall, JWTs provide a secure and efficient way for transmitting and verifying information between parties, making them widely used in modern web applications.
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

15. Do you know if caching is applied in the rest api's that you test? How do you test caching in api's
16. How do you test expired or invalid tokens in your API's?
17. 𝐇𝐨𝐰 𝐝𝐨 𝐲𝐨𝐮 test 𝐮𝐬𝐞𝐫 𝐫𝐨𝐥𝐞𝐬 𝐚𝐧𝐝 𝐩𝐞𝐫𝐦𝐢𝐬𝐬𝐢𝐨𝐧𝐬 𝐢𝐧 𝐲𝐨𝐮𝐫 𝐀𝐏𝐈, tell us some of the best practices that you follow for testing such user permissions in API's.
18. Have you heard of the term rate-limiting? Can you explain when should we use rate limiting in API's
19. Do you generate reports of the api tests that you run? What are some key attributes to include in the report.
20. Have you heard of the term api gateway? What does it do?
21. What is the difference in path param and query params?
22. Do we need to use POJO class and serialisation in RestAssured, when we can directly send request body in the form of String?
23. Can I use cookies with rest assured - if so how can I set a cookie for a domain?
24. Do you know what is a HEAD request? Can you think of a scenario when HEAD request would be needed.
25. Is POST a cacheable method? Is PUT a cacheable method?
26. Difference between api virtualisation and mocking?
27. What is JSON Schema? Can I use same json schema for validating the response of two different api's?
28. What is API caching and in how many ways can we cache api response.
#strategy #content #testing

### Lesson 16
We have been on a journey to implement Continuous testing (CI & CD) as a tech culture and mindset along with the required tooling .
Majorly for backend , there is a CI pipeline (15-20 mins) implemented on Jenkins which runs on a webhook triggered from bitbucket on every PR creation. This covers build, deploy, and test phase of the pipeline for early feedback using docker/k8s/cloud servers/etc.
Post that, there is a CD pipeline (2-3 hours) which has UI/API/Security/Performance checks before the final production deployment.



