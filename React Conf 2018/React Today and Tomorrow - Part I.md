> * 原视频地址：[React Today and Tomorrow and 90% Cleaner React with Hooks](https://youtu.be/dpw9EHDh2bM)
> * 演讲者:  [Sophie Alpert](https://mobile.twitter.com/sophiebits)
> * 英文字幕出自：YouTube 机器翻译
> * 英文校对：[Ivocin](https://github.com/Ivocin)
> * 排版：[Ivocin](https://github.com/Ivocin)



# React Today and Tomorrow - Part I

Good morning. see ... ya ... there ... Uh, hello everyone, welcome to React Conf. I'm really excited to be here. I'm really excited for the stuff that we have to announce, uh, for you this week. 

Um, my name is Sophie Alpert at sophiebits on the Internet. I manage the React core team at Facebook.

## React Today

By any better that you use React is doing great. Our npm downloads are up 70% year-over-year. The Chrome Dev Tools extension for React Dev Tools has been installed by one and a quarter million developers. 

And I can show you this list of companies that's using React. Although at this point, it's so long, it's kind of hard to tell how much it changes each year. 

For another point of data, we can look at Google Trends, which shows web search traffic. So it searches for React continue to go up. Hopefully that means more people are using React not that it's getting more confusing. (Laughing) 

Um, but, uh, for a point of comparison, we can look at, um, jQuery which we have just passed for the first time in history. (Cheer & Applause) But this also shows that we have a lot more room to grow. 

Um, I was procrastinating when I was writing this talk. So I was urious to see what else React is more popular than. Oops. (Laughing) Spoiled my joke. Um, but React is more popular ,I found out, than renewable energy. (Laughing) It's also more popular than orange juice. (Laughing) Just think about how common orange juice is, right. And not only that it's more popular than renewable energy and orange juice put together. So I think we have a lot to be proud of. 

## React's Mission

But, um, but enough about these numbers, uh, what I really like to talk about today is our mission with React. Um, ever since React was released in 2013, our overarching goal, our primary mission has been to make it easier to build great UIs. And so when we're adding new features, we always try to be very deliberate. We want to consider a bunch of things when deciding whether to add a new API. If it makes it possible to do something you couldn't do before. If it can dramatically simplify the code around React in your components and libraries so that you all have less work to do and users have less code to download, that's a win. Or if it helps encourage best practices like code splitting, if we make it easier to code split your app into multiple bundles, then our hope is that your apps will end up being faster. So that's why we add things like React.lazy which we announced two days ago. You might have seen it. But thinking about this mission, make it easier to great ... to build great UIs.

There are a lot of different ways that we approach this. One way we do this is trying to simplify things that are hard. If you saw Dan Abramov's talk from JS Conf Iceland, then you saw a sneak peek at "Suspense", which is our idea about how to dramatically simplify what's required to do data fetching, code splitting and any kind of async data dependencies in your app. 

Now another way we try to improve React is by focusing on performance. If your app runs faster, your users are going to enjoy using it more. Conversely, if your app is laggy, if your app is junky, then there your users aren't gonna have a great time. So we try to spend time on making React itself faster, because if React is faster out of the box, you all need to spend less time optimizing your own code. 

One recent performance related effort that Dan also talked about in Iceland, as what we call "Time Slicing". Uh, this is going to let you make sure that the most important renders in your app are processed first, in order to unblock the main thread and make your apps faster. 

And a third angle that we approach our mission from is Developer tooling to help you debug and understand your app. From the start, React has included developer friendly warnings to help, uh, point out problems before you might otherwise notice them.

And we've had the React Dev Tools extension which lets you inspect and debug your component trees. And in React 16.5, we introduced a new Profiler. It's a second ... (I don't know what's up with this clicker) ... A second tab their, profiler tab that helps you understand what's happening in your app and optimize it. 

So Suspense, Time Slicing and the Profiler are three of the new features that we've been working on over the last year. We're really excited to tell you more about them.But that's actually not what I'm here to talk about. You're gonna have to wait till Andrew and Brian's talk tomorrow morning to hear about that.

## What in React still sucks

Today I want to take a step back and focus on something else. What I like to ask is what in React still sucks. And I have three problems that I would like to talk through. 

### Reusing logic

The first one is reusing logic between multiple components. In React our main building block for our applications is a component, and components form the foundation of the two main patterns for sharing code in React apps between components which are higher-order components and render props. 

Both of these patterns are great for some cases, but they also come with a significant downside. You need to restructure your app anytime you want to pull one of these in in more complicated examples. This leads to what I call wrapper hell.

Uh, most of us have seen component trees that look something like this. (Screaming & Laughing)  And the the nesting you end up with makes it difficult to follow the data flow through the app. It would be really nice if there was some way to reuse this sort of stateful logic without needing to change the component hierarchy, right.


### Giant components

The second problem I would like to talk about is giant components whose logic is just sort of a tangled mess. When you look at a thousand line React component chances are the logic is going to be split across a lot of different lifecycle methods in a way that's pretty difficult to follow. 

Let's look at an example. Let's say we have a class component, and in its component did mount method it does a few different things: it subscribes to a datastore, it sends off a network requests and it starts a timer.

Well, if we look at the component will unmount method, then we're going to see basically the exact three opposite
things: it needs to unsubscribe from the store, it needs to cancel that network request and it needs to stop the timers.

And when it comes to implementing component did update, the logic tends to get even trickier because you need to compare the old and new props and (ur, and and) also mirror it again the same tasks that you have in your other lifecycle methods. 

Uh, in this example, each call here is just one line so this is actually a lot simpler than what you normally see in your components. In real-world components you often end up with an even more tangled mess, because each (ur, each) individual task has to be split across different lifecycle methods, that makes it hard to tell if, for instance, you forgot to clean up one of the resources when you're unmounting your component. It's pretty hard to see that from the code. 

### Confusing Classes

And the third thing that sucks is the class, ur, understanding classes in JavaScript can be pretty tricky, and today we require you to use class omponents in order to access state and lifecycles. 

If you've ever taken a function component and converted it to a class to add some state, you know that there's a fair amount of boilerplate that's required in order to just define a class component. Most beginners and many experienced devs also tell us that the way binding and this work in classes is pretty confusing. It's annoying to
have to think about.

And we also frequently hear that people don't exactly know when to use function components partly because there's always this fear that you're gonna have to convert it to a class later anyway. And so you're like should I ,should I do it now? I don't know.

And so I claim classes are hard for humans,  uh, but it's not just humans,  I claim the classes are also hard for machines. If you ever looked at a minified component file, you'll see that all the method names are still unminified. And that if you have a method that's completely unused, it doesn't get stripped out. That's because it's hard to tell at compile time exactly how all the methods fit together.

We also found that classes make it difficult for us to implement hot reloading reliably. And finally when we
were prototyping an optimizing compiler to improve the performance of React components, we found that classes can
encourage some patterns that make it a lot harder for compilers to optimize.

### Conclusion

So here are the three problems that we have: reusing logic, giant components, and (ur, and) classes. So reusing logic because you often end up with this wrapper hell. Giant components because you have the logic split across different lifecycles. And classes which are difficult for both humans and machines. 

So we think we have a solution that can help with all three of these. We're really excited to share it with you to tell you more about it. I want to welcome up Dan Abramov.
