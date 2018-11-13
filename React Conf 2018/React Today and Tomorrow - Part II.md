> * 原视频地址：[React Today and Tomorrow and 90% Cleaner React with Hooks](https://youtu.be/dpw9EHDh2bM)
> * 演讲者:  [Dan Abramov](https://twitter.com/dan_abramov)
> * 英文字幕出自：YouTube 机器翻译
> * 英文校对：[Ivocin](https://github.com/Ivocin), [程序媛_小发](https://github.com/Augustwuli)
> * 排版：[Ivocin](https://github.com/Ivocin)



# React Today and Tomorrow - Part II

Hi. Ah, my name is Dan, I work on the React Team and this is my first time in React Conf. (Applause) 

## Promblems Today

And so Sophie talked about these problems that I think most of you have encountered in React development. And of course we could approach these problems one by one so we could try to solve them in isolation. But it seems like solving one of them makes some other one worse.

So for example, if we try to solve the wrapper hell by putting more logic into components themselves, then our components become larger and harder to refactor. And then on the other hand, if we try to split the components apart into smaller pieces and reuse those, then we end up with more nests than in the component tree and we get the wrapper hell again. And finally in either case we have all the confusion that comes with the classes. 

So we think that this is because these are not three separate problems. We think that this is, ah, these are three symptoms of one problem. And the problem is that React does not provide a simpler, smaller, lightweight primitive to add state or lifecycle than a class component. 

And so once you add a class component, you can split it up further without introducing the wrapper hell. And in fact it's not a new problem. So if you use React for like more than a couple of years, you might remember then when React came out it actually included a solution to this problem. Ah, it was mixins. So mixins allow you to reuse some methods between classes and this way you wouldn't have all these wrappers. 

So should we add mixins back to React? [Yeah ... No...] That's right, no no, we're not gonna do that. Ah, I mean the codes using mixins the rounds, it's not like, it's not broken. But we don't encourage using mixins in React. And if you curious why there is a blog post, uh, that we wrote code called [*Mixins Considered Harmful*](https://reactjs.org/blog/2016/07/13/mixins-considered-harmful.html) on the React Blog, where we explain that we think in our experience, the problems that mixins create are worse than the problems that they solve. Ah, so that's why we don't encourage using mixins. 

## We have a proposal

So perhaps we could just can't solve this problem maybe its inherent to the React component model. Maybe we should just accept it. (Laughing) Or maybe in there is a, perhaps there is a different way we could write our components that doesn't suffer from either of these problems. And that's what I'm going to talk about today. 

But before I start I want to touch a little bit on the way we approach making changes and additions to React which is a year ago, we set up an RFC process, so RFC stands for request for comments, and it means that whenever we want to make or somebody else wants to make a substantial change or addition to React, they need to write up a proposal with detail in the motivation and the detailed design of how this will work.

And so that's what we're going to do. We're excited to announce that we are ready to present a proposal for how we can solve these three problems.

And importantly this proposal does not have breaking changes or deprecations in it. It is strictly additive, it is opt-in, and it adds some new APIs which happens when they try to solve problems. And we would love to hear your feedback on this proposal which is why we're, ur, we're going to publish it today. 

And we thought about many ways we could share this proposal, so maybe we just like write off in there ... write up an RFC and post it. Um, but since we were going to run React Conf anyway, we just decided to, ah, just show it here.


## Demo

And we're going to demo. [Applause]

It already mirrors the displays. Sorry, technical glitch. Ur, can somebody who understands projectors help me. [Laughing] Ur, can I make it mirror my desktop? Please. (I can do it) Yeah. [Laughing] Okay but it doesn't show on the screen, I don't see anything. [Laughing] That's, that's the problem that I had.  [Applause] Okay, disaster averted. [Laughing] Alright, um, let me check the font size a little bit. Do you see this file? [Yeah] All right.

### A familiar class component demo

So, ah, here is a simple React component, ah, that just it is a row, this is just some styling, and it it renders a person's name. And so let's say that we want this name to be editable. So how do you do it in React normally? Well, like if we want to add an input there we need to return this component into a class, add some local state to it, and let that state drive the input. So that's exactly what I'm going to do, uh, that's what we do today.

So I'm gonna do the export default class Greeding extends React component. And so I'm going to use only stable JavaScript syntax. So constructor props, super props. This the state, going to initialize, ur, name to Mary here. And I'm going to declare a render method and copy and paste this. Sorry. Okay. 

And so I want this to be instead of just rendering the name, I want this render an input. So I'm replacing this by an input, and the value of the input is this that state that name. And if I make a change I want to call this dot handleNameChange that's going to be my change caller. And I'm going to declare it here, when the name changes we call setState like we normally do. And we set the name to e dot target dot value. Right. 

So now if I edit the ... [TypeError In Page] Okay so I I need to bind ... [Laughing] Sorry I need to bind the events on there. Okay so now I can actually edit it and it works. 

So a familiar class component is, uh, if you work with React probably right a lot of those. 


But let's take a step back. What if we didn't have to write a class when we wanted to use state. So I'm not sure how that's gonna work. But I'll just start with what I know, I want to render an input. So I'm gonna put an input here. And the input has a value and that value is the current name, so I'll just pass name. I don't know where to get name from. So it doesn't come from props, uh, I'll just declare it, and I don't know, I'll fill it in later. 

Uh, it's gonna have a change caller there as well, so I'm going to declare onChanged handleNameChange. And I'm adding a function here takes an event. And then here I want to tell React to set the name to something, but again I'm not sure how to do that from a functional component. So I'll just call something called setName. With the current input value. And I'll
just declare it here.

All right, so these two things they're closely related, right. So one of them is the current value of the name state variable and the other is a function that lets us set the name state variable. And so because these things are closely related, I'm actually going to put them together as a pair of values. 

So I'm going to get them together from somewhere. So where do I get them from? From react local state. So how do I use react local state from a function component? Well, what if I could just use state. And past the initial state to specify it.

Let's see if this works. Yeah, it works.

[Applause & Cheers]