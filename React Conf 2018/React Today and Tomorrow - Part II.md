> * 原视频地址：[React Today and Tomorrow and 90% Cleaner React with Hooks](https://youtu.be/dpw9EHDh2bM)
> * 演讲者:  [Dan Abramov](https://twitter.com/dan_abramov)
> * 英文字幕出自：YouTube 机器翻译
> * 英文校对：[Ivocin](https://github.com/Ivocin)
> * 排版：[Ivocin](https://github.com/Ivocin)



# React Today and Tomorrow - Part II

Hi. Ah, my name is Dan, I work on the React Team and this is my first time in React Conf. (Applause) 

And so Sophie talked about these problems that I think most of you have encountered in React development. And of course we could approach these problems one by one so we could try to solve them in isolation. But it seems like solving one of them makes some other one worse.

So for example, if we try to solve the wrapper hell by putting more logic into components themselves, then our components become larger and harder to refactor. And then on the other hand, if we try to split the components apart into smaller pieces and reuse those, then we end up with more nests than in the component tree and we get the wrapper hell again. And finally in either case we have all the confusion that comes with the classes. 

So we think that this is because these are not three separate problems. We think that this is, ah, these are three symptoms of one problem. And the problem is that React does not provide a simpler, smaller, lightweight primitive to add state or lifecycle than a class component. 

And so once you add a class component, you can split it up further without introducing the wrapper hell. And in fact it's not a new problem. So if you use React for like more than a couple of years, you might remember then when React came out it actually included a solution to this problem. Ah, it was mixins. So mixins allow you to reuse some methods between classes and this way you wouldn't have all these wrappers. 

So should we add mixins back to React? [Yeah ... No...] That's right, no no, we're not gonna do that. Ah, I mean the codes using mixins the rounds, it's not like, it's not broken. But we don't encourage using mixins in React. And if you curious why there is a blog post, uh, that we wrote code called [*Mixins Considered Harmful*](https://reactjs.org/blog/2016/07/13/mixins-considered-harmful.html) on the React Blog, where we explain that we think in our experience, the problems that mixins create are worse than the problems that they solve. Ah, so that's why we don't encourage using mixins. 

So perhaps we could just can't solve this problem maybe its inherent to the React component model. Maybe we should just accept it. (Laughing) Or maybe in there is a, perhaps there is a different way we could write our components that doesn't suffer from either of these problems. And that's what I'm going to talk about today. 

But before I start I want to touch a little bit on the way we approach making changes and additions to React which is a year ago, we set up an RFC process, so RFC stands for request for comments, and it means that whenever we want to make or somebody else wants to make a substantial change or addition to React, they need to write up a proposal with detail in the motivation and the detailed design of how this will work.

And so that's what we're going to do. We're excited to announce that we are ready to present a proposal for how we can solve these three problems.