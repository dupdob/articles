# opening an issue (in GitHub)
### Why is that even a topic?
I often explain people the first and smartest way to contribute to OSS is by opening an issue. Why? I am glad you ask (even if I was the one raising this):

1. an issue IS feedback, and feedback is a gift
2. it proves you use the OSS
3. it proves you care for the OSS
4. it can provide valuable insight in how users interact with the OSS
5. it gives you an opportunity to improve YOUR experience with the OSS

This last point is important: as an OSS maintainer, I know the vast majority of users remain silent; as such, they give me no information on how they use it, what they like and what they don't like; on what I should spent some time upon and what could/should be discarded.

### Why Me? Can't somebody else do it for me?
TLDR; NO

Longer version: No, because nobody else will!

- You may be the first one to encounter the problem, because you used the library in some novel, unexplored way without even realizing it.
- Opening an issue is work (see below)
- Other people have a different context, so they will express the issue differently (again, see points #2 and #3 below).
- Other people have a different agenda, so they will open a very different issue

### Why do you even write about this?
Again, I am glad you ask. Because I think people should open more issues (remember, issue is feedback), and I think people may be afraid to do it because they fear apearing stupid, or that there problem is not important.
Also, I prefer to work on well described issues, but sadly most issue are hard to work with because details are missing. But let's focus on what constitutes a good issue.

### What are the attributes of a good issue?
**Disclaimer**: these are my criteria. I tend to think many will agree with those; I list them in decreasing order of importance, and I guess this order is debatable, but this is mine and I stand by it.
Lets discuss issues first, features and pull requests afterwards.

===
### A good issue has
1. reproduction details:
  1. your context: configuration, environment....
  2. ideally sample code that exhibits the issue.
  3. alternatively, reproduction steps.
  4. log files, if any.
1. a precise description of symptoms: you have to be as specific as you can. You should give precise error messages, exception (incl. stack trace).
1. a description of the impact: you must express **why this is a problem for you**. Is it blocking you somehow?
1. a short description of what should have happened from your point of view.
1. be ok to provide additional details.
1. keep a respectful tone all along the exchange.

====
The response time if directly related to the quality of the input. If I can reproduce the issue and understand why this is a problem for you, I will probably address it as soon as possible.

\#3 may come as a surprise to some of you; but bear in mind that an OSS maintainer has many users as well as her/his own agenda, i.e. a vision for the project, so providing motivation for a change is important. 

\#2 is more important that you may think. More often than not, you are reporting an obvious issues, such as an unexpected exception, are'nt you? But beware of the implicit, **obviousness
is in the mind of the reporter**; maybe the maintainer will consider that what you are/were trying to do is outside the scope of the project, or maybe the exception is not unexpected from her/his point of view. One last thing on #2: it may be the most discussed part of the issue, as you will have to reach an agreement regarding the propoer behaviour.

**Maybe you find some items are missing from my list**. Feel free to comment, your feedback will be most welcome. But let me try to address some candidates.

What about providing an analysis of the likely culprit ? Indeed, maybe you spent time trying to figure out what was hapenning so why not sharing your hindsights with the team.
But b: what I want his a reproduction scenario, not some hints regarding what may be causing the issue. Without any reproduction scenario, I will not be able to devise a unit test to prevent regression. Maybe I will not be able to manualy test and confirm the issue has been fixed. Furthermore, I need to be convinced to spend some of my free time to help you; the least you can do is tell me hwo much it would help you.

What about providing a fix as pull request for this? well, this can be a bonus point if you got everything else on the list. Indeed, I still need some reproduction details, so I can be convinced that the PR will fix something; I also still need some explanation on why this is an issue for you; because, one man's bug can be some other man's feature! Putting it in another way, you will introduce a change of behaviour which may introduce (other) bugs (see [Hyrum's law](https://www.hyrumslaw.com/)).


##### Want to see what I consider to be good issues?
Here are some:

* NFluent: [IsEquivalentTo for Dictionaries does not work correctly](https://github.com/tpierrain/NFluent/issues/334)
* NFluent: [WithCustomMessage does not give a custom message when being used with IsTrue()](https://github.com/tpierrain/NFluent/issues/313)
* Stryker-mutator: [Options to include or exclude LinqExpressions on LinqMutator](https://github.com/stryker-mutator/stryker-net/issues/1546)

Here is a poor one:

* [Handle recursion loop in HasFieldsWithSameValues](https://github.com/tpierrain/NFluent/issues/148) (yes, it is mine).

### What about bad issues?
Unless the issue system is used for sexist, racist comments or some other form of harassment, there is no such thing as a "bad issue". If an issue is lacking some elements of my list, it does not make it a 'bad' issue; it mostly impacts the delay (and probability) I will address it.


1. Symptoms are important; in the best case, symptoms may be sufficient to help me identify the cause. But details are important: **"it crashes on my project" does not qualify as a detailed description**.
2. if you can not give reproduction information, I am fraid you're on a very bad start: if I can't reproduce the issue, how can I ever fix it? You may have to share some code, or event a whole project to help me help you. If you can't, I may ask for extra details, but there is a high chance I will simply wait to see if someone else report the same issue with the required information.
3. if you fail to describe the impact, there is a high chance I will underestimate the priority. 
4. if you do not describe the expected behaviour, there is a significant risk of misunderstanding, resulting in a 'fix' you do not consider a fix, as it does not bring what you needed in the first place.

Note that in general, you should expect the OSS teams to request for details on those items when they are not clear enough, but as I said earlier, this increases the time to resolution and it increases the risk of the issue not being picked up.

### What about the don'ts
There are a few things you should keep in mind:

1. **Do not press or demand anything to the maintainers**; just ask politely and stress this is important to you and why. I am afraid they do not owe you anything. Even if your problem is critical for your company! They work on their free time for the community, as such they deserve your respect and their freedom of choosing on when and on what they work.
2. **Do not open a pull request without opening (at least) one (or more) issue(s)** describing the problem(s) you saw and that your PR fixes. Often an issue leads to discussion on the best strategy to fix it, or weither it is a bug or a feature; so opening a PR without an issue will appear as pushy to say the least.
3. **Complain publicly about the project** because your issue has been closed. See #1. Note that opening a PR to fix it your self is probably not a good idea either. If this issue is really important to you, I suggest you discuss with others about it and contemplate finding the solution in some OSS project.
