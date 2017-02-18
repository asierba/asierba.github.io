---
layout: post
title:  "Formal code reviews"
---

Simply put: I don't like formal code reviews.

What do I mean with formal code review?

 A developer picks a new task to work on, creates a branch from master, does all the necessary work in that branch and when she is finished creates a "request" to merge those changes to master - usually known as *pull request*. Then a second developer comes and checks the code changes in the pull request and adds some comments to it so the initial developer can amend parts of the code where she might have missed something, done something wrong or whatever. Then she can make more code changes and update the pull request. When both parties are happy the branch is merged to master and everybody is happy!

My main thing against this kind of code review is that they tend to **slow down development a lot**! When a developer finishes his work he has to create the pull request and then wait for somebody else to pick that up and go through it. Probably everybody else in the team is already working on something else. So the options are: you either interrupt somebody else's work so the code review can be done or you go away and start doing something else. If you do the later your work will get interrupted once the code review is completed and you will have to go through the feedback. Either way there is **waiting and context switching** involved. More importantly, the [lead time](https://en.wikipedia.org/wiki/Lead_time) of development work is going to get increased - is going to take longer for features to get from ready to done.

Things could get worse. Some people love adding a lots of comments in the code review. I don't see the point of this. If you see many issues with the code you are either too picky or there is a bigger problem in the team.. Maybe there is a communication issue? It could be that it is not clear which pattern we should be using in the code, which style should we use in the code or something similar.

My simple **solution is just to pair program**. Pair programming is a synchronous code review. Both the person developing the feature and the reviewer are sitting at the same computer at the same time. The comments about the code will happen at the moment, discussions will be quick and effective. And once the work is done it could just go to master! No more waiting times! :)

I strongly believe that extreme programming is the best way to develop software. Extreme programming doesn't talk about code reviews, but it mentions pair programming as core practice. I don't think that's coincidence.

The problem is that, sadly, many teams don't believe in pair programming or extreme programming as much as I do. 

If you don't want to replace formal code reviews with pair programming I would say to do the code reviews as fast as possible. That's the only way I found them effective in the past. Just add a few comments and browser the code quickly. Just focus on the important bits. You don't have to look for small details in the code! If there are small issues you can just fix them later on when you are working on that area. Code is easy to change in those cases. 
If after all, you still find many issues maybe you should be sitting with that person and pair with him for a bit so you could teach how it should be done! :)