# Yet Another Post On Code Coverage

A few days ago, @unclebob tweeted about code coverage...

<blockquote class="twitter-tweet" data-partner="tweetdeck"><p lang="en" dir="ltr">The only test coverage goal that makes any sense is 100%. It’s an asymptotic goal. You’ll likely never get there. But you should never stop trying.</p>&mdash; Uncle Bob Martin (@unclebobmartin) <a href="https://twitter.com/unclebobmartin/status/1205922909350293505?ref_src=twsrc%5Etfw">December 14, 2019</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

As often with Uncle Bob and always this this topic, this ingnited a thread of protests against this **stupid (?) idea**.

# Stupid, really?
I am still strongly opinionated toward 100% coverage. Alas, each time I voice this opinion, everybody is keen to explain to me why I am wrong and nobody bothers to ask me why I believe in it.

That's why I have written a bunch of posts on this topic along the year:

- [Unit testing, you’re doing it wrong](Unit testing, you’re doing it wrong)

- [Quick Rex about 100% code coverage](https://many-cores.com/2018/12/06/unit-testing-youre-doing-it-wrong/)

- [The secret for 100% test coverage: remove code
](https://many-cores.com/2014/02/21/the-secret-for-100-test-coverage-remove-code/)

On the other hand, I never took the time to comment/challenge the negative responses I get. Well, this situation has come to an end, as I intend to use this post to respond to these.

Or to be more precise, I will address the negative comments Uncle Bob got. Not because he needs any help from me, but he got a lot of them, and pretty good ones, as comments go.

## Usual warning
Any test first practice is difficult!

Writing tests is difficult.

**TDD** is a difficult beast that takes years to master. I have been TDDing most of my code for the past 15 years and I am still learning everyday. So it is ok to make mistakes or to fail to grasp some of the benefits.

# Let's go
I regrouped comments by theme for easier reading.

## Some code is not worthy of unit testing

<blockquote class="twitter-tweet" data-conversation="none" data-theme="light"><p lang="en" dir="ltr">If you skip standard getters and setters and the noarg constructor, this is certainly feasible is my experience.</p>&mdash; GeborenAtheïste (@GeborenAtheiste) <a href="https://twitter.com/GeborenAtheiste/status/1205938329344188417?ref_src=twsrc%5Etfw">December 14, 2019</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

We have a nice list here...
Well, either the setters/getters/constructor/you-name-it are used in some useful code, then they are tested. Or they are not tested and then you can simply **remove them**.


<blockquote class="twitter-tweet" data-conversation="none" data-theme="light"><p lang="en" dir="ltr">Including private constructors of Java classes designed to be uninstantiable? Please...</p>&mdash; jub0bs (@jub0bs) <a href="https://twitter.com/jub0bs/status/1206620747541753856?ref_src=twsrc%5Etfw">December 16, 2019</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

This one is relevant: 



### To Comment
<blockquote class="twitter-tweet" data-conversation="none" data-theme="light"><p lang="en" dir="ltr">Including private constructors of Java classes designed to be uninstantiable? Please...</p>&mdash; jub0bs (@jub0bs) <a href="https://twitter.com/jub0bs/status/1206620747541753856?ref_src=twsrc%5Etfw">December 16, 2019</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

<blockquote class="twitter-tweet" data-conversation="none" data-theme="light"><p lang="en" dir="ltr">I am going to make a bold claim: it is important to make sure you kill 100% mutants in your repo. It is a perfectly achievable goal, and we&#39;ve been accomplishing it daily. But you need to keep an eye on those mutants and don&#39;t let them multiply like rabbits.</p>&mdash; Alex Bunardzic (@alexbunardzic) <a href="https://twitter.com/alexbunardzic/status/1205934497214058496?ref_src=twsrc%5Etfw">December 14, 2019</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>	

<blockquote class="twitter-tweet" data-conversation="none" data-theme="light"><p lang="en" dir="ltr">Is it sensible to test plain crud methods without any logic? And what about mocking you mock your data source and expect what you set to your mock...</p>&mdash; Oğuzhan (@oguzhanoguz) <a href="https://twitter.com/oguzhanoguz/status/1206304476023447552?ref_src=twsrc%5Etfw">December 15, 2019</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

<blockquote class="twitter-tweet" data-conversation="none" data-theme="light"><p lang="en" dir="ltr">Is it sensible to test plain crud methods without any logic? And what about mocking you mock your data source and expect what you set to your mock...</p>&mdash; Oğuzhan (@oguzhanoguz) <a href="https://twitter.com/oguzhanoguz/status/1206304476023447552?ref_src=twsrc%5Etfw">December 15, 2019</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

<blockquote class="twitter-tweet" data-conversation="none" data-theme="light"><p lang="en" dir="ltr">This sort of silly advice is what leads to testing implementation instead of behavior.<br><br>When you shoot for a percentage (especially one so easy to fake), you wind up with brittle tests and a maintenance nightmare.<br><br>Test the code that matters. Not your toString().</p>&mdash; Rick Busarow (@RBusarow) <a href="https://twitter.com/RBusarow/status/1205924870728667137?ref_src=twsrc%5Etfw">December 14, 2019</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

<blockquote class="twitter-tweet" data-conversation="none" data-theme="light"><p lang="en" dir="ltr">If you have tested all the behaviour then any test you write next is too focused on implementation. Leads to brittle tests and adds zero value unless you are being paid for each line of code written</p>&mdash; Lewis Reilly (@lews0r) <a href="https://twitter.com/lews0r/status/1205927168712294400?ref_src=twsrc%5Etfw">December 14, 2019</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
<blockquote class="twitter-tweet" data-conversation="none" data-theme="light"><p lang="en" dir="ltr">If you have tested all the behaviour then any test you write next is too focused on implementation. Leads to brittle tests and adds zero value unless you are being paid for each line of code written</p>&mdash; Lewis Reilly (@lews0r) <a href="https://twitter.com/lews0r/status/1205927168712294400?ref_src=twsrc%5Etfw">December 14, 2019</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>


<blockquote class="twitter-tweet" data-conversation="none" data-theme="light"><p lang="en" dir="ltr">But management by numbers is anti-pattern because you&#39;ll have to cut open the guts of implementation. You will spend 99% of the time on a test that provide 1% of value. You will always be scared of refactoring, cause any touch to code will break multiple tests.</p>&mdash; EresDev (@EresDev) <a href="https://twitter.com/EresDev/status/1205940320011194368?ref_src=twsrc%5Etfw">December 14, 2019</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

<blockquote class="twitter-tweet" data-conversation="none" data-theme="light"><p lang="en" dir="ltr">But management by numbers is anti-pattern because you&#39;ll have to cut open the guts of implementation. You will spend 99% of the time on a test that provide 1% of value. You will always be scared of refactoring, cause any touch to code will break multiple tests.</p>&mdash; EresDev (@EresDev) <a href="https://twitter.com/EresDev/status/1205940320011194368?ref_src=twsrc%5Etfw">December 14, 2019</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

<blockquote class="twitter-tweet" data-conversation="none" data-theme="light"><p lang="en" dir="ltr">I&#39;d normally say that 100% coverage is bad, because it forces to test getters and setters as well. And then it dawned on me that these tests would be useful if someone adds some logic to getter/setter which otherwise would go untested</p>&mdash; Markus Karileet (@MarkusKarileet) <a href="https://twitter.com/MarkusKarileet/status/1206101017777115136?ref_src=twsrc%5Etfw">December 15, 2019</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>