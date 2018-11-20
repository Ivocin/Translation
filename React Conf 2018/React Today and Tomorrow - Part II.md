> * 原视频地址：[React Today and Tomorrow and 90% Cleaner React with Hooks](https://youtu.be/dpw9EHDh2bM)
> * 演讲者:  [Dan Abramov](https://twitter.com/dan_abramov)
> * 英文字幕出自：YouTube 机器翻译
> * 英文校对：[Ivocin](https://github.com/Ivocin), [程序媛_小发](https://github.com/Augustwuli)
> * 排版：[Ivocin](https://github.com/Ivocin)



# React Today and Tomorrow - Part II

Hi. Ah, my name is Dan, I work on the React Team and this is my first time in React Conf. (Applause) 

## Promblems Today

And so Sophie talked about these problems that I think most of you have encountered in React development. And of course we could approach these problems one by one, so we could try to solve them in isolation. But it seems like solving one of them makes some other one worse.

So for example, if we try to solve the wrapper hell by putting more logic into components themselves, then our components become larger and harder to refactor. And then on the other hand, if we try to split the components apart into smaller pieces and reuse those, then we end up with more nests than in the component tree and we get the wrapper hell again. And finally in either case we have all the confusion that comes with the classes. 

So we think that this is because these are not three separate problems. We think that this is, ah, these are three symptoms of one problem. And the problem is that React does not provide a simpler, smaller, lightweight primitive to add state or lifecycle than a class component. 

And so once you add a class component, you can split it up further without introducing the wrapper hell. And in fact it's not a new problem. So if you use React for like more than a couple of years, you might remember then when React came out it actually included a solution to this problem. Ah, it was mixins. So mixins allow you to reuse some methods between classes and this way you wouldn't have all these wrappers. 

So should we add mixins back to React? (Yeah ... No...) That's right, no no, we're not gonna do that. Ah, I mean the codes using mixins the rounds, it's not like, it's not broken. But we don't encourage using mixins in React. And if you curious why there is a blog post, uh, that we wrote code called [*Mixins Considered Harmful*](https://reactjs.org/blog/2016/07/13/mixins-considered-harmful.html) on the React Blog, where we explain that we think in our experience, the problems that mixins create are worse than the problems that they solve. Ah, so that's why we don't encourage using mixins. 

## We have a proposal

So perhaps we could just can't solve this problem maybe its inherent to the React component model. Maybe we should just accept it. (Laughing) Or maybe in there is a, perhaps there is a different way we could write our components that doesn't suffer from either of these problems. And that's what I'm going to talk about today. 

But before I start I want to touch a little bit on the way we approach making changes and additions to React which is a year ago, we set up an RFC process, so RFC stands for request for comments, and it means that whenever we want to make or somebody else wants to make a substantial change or addition to React, they need to write up a proposal with detail in the motivation and the detailed design of how this will work.

And so that's what we're going to do. We're excited to announce that we are ready to present a proposal for how we can solve these three problems.

And importantly this proposal does not have breaking changes or deprecations in it. It is strictly additive, it is opt-in, and it adds some new APIs which happens when they try to solve problems. And we would love to hear your feedback on this proposal which is why we're, ur, we're going to publish it today. 

And we thought about many ways we could share this proposal, so maybe we just like write off in there ... write up an RFC and post it. Um, but since we were going to run React Conf anyway, we just decided to, ah, just show it here.


## Demo

And we're going to demo. (Applause)

It already mirrors the displays. Sorry, technical glitch. Ur, can somebody who understands projectors help me. (Laughing) Ur, can I make it mirror my desktop? Please. (I can do it) Yeah. (Laughing) Okay but it doesn't show on the screen, I don't see anything. (Laughing) That's, that's the problem that I had. (Applause) Okay, disaster averted. (Laughing) Alright, um, let me check the font size a little bit. Do you see this file? (Yeah) All right.

### A familiar class component demo

So, ah, here is a simple React component, ah, that just it is a row, this is just some styling, and it it renders a person's name. And so let's say that we want this name to be editable. So how do you do it in React normally? Well, like if we want to add an input there we need to return this component into a class, add some local state to it, and let that state drive the input. So that's exactly what I'm going to do, uh, that's what we do today.

So I'm gonna do the export default class Greeding extends React component. And so I'm going to use only stable JavaScript syntax. So constructor props, super props. This the state, going to initialize, ur, name to Mary here. And I'm going to declare a render method and copy and paste this. Sorry. Okay. 

And so I want this to be instead of just rendering the name, I want this render an input. So I'm replacing this by an input, and the value of the input is `this.state.name`. And if I make a change I want to call `this.handleNameChange`, that's going to be my change caller. And I'm going to declare it here, when the name changes we call setState like we normally do. And we set the name to `e.target.value`. Right. 

So now if I edit the ... (TypeError In Page) Okay so I I need to bind ... (Laughing) Sorry I need to bind the events on there. Okay so now I can actually edit it and it works. 

So a familiar class component is, uh, if you work with React probably right a lot of those. 

### Can we use function component instead?

But let's take a step back. What if we didn't have to write a class when we wanted to use state. So I'm not sure how that's gonna work. But I'll just start with what I know, I want to render an input. So I'm gonna put an input here. And the input has a value and that value is the current name, so I'll just pass name. I don't know where to get name from. So it doesn't come from props, uh, I'll just declare it, and I don't know, I'll fill it in later. 

Uh, it's gonna have a change caller there as well, so I'm going to declare onChange `handleNameChange`. And I'm adding a function here takes an event. And then here I want to tell React to set the name to something, but again I'm not sure how to do that from a functional component. So I'll just call something called setName. With the current input value. And I'll
just declare it here.

All right, so these two things they're closely related, right. So one of them is the current value of the name state variable and the other is a function that lets us set the name state variable. And so because these things are closely related, I'm actually going to put them together as a pair of values. 

So I'm going to get them together from somewhere. So where do I get them from? From React local state. So how do I use React local state from a function component? Well, what if I could just use state. And past the initial state to specify it.

Let's see if this works. Yeah, it works.

(Applause & Cheers)

So let's, let's compare the two approaches. So on the left we have a familiar class component. This state has to be an object. Uh, we bind some event handlers so that we can access. This inside the event handler for this dot setState. When we call setState it actually doesn't just set the state that merges, the state are the argument into the state object. And then when we want to access the state which is this dot state dot something. 

So in the example on the right, we don't need to access this dot state dot something. Because the name state variable is already available in the function. It's just the variable. And similarly when we need to set the state, we don't need to access this dot something. Because the function that let us set the name is also available in the scope. So what is `useState` exactly? UseState is a hook. **A hook is a function provided by React that lets you hook into React features from your function components**. And `useState` is the first hook that we're going to take a look at today, but there are a few more. So we're going to see them later.

### Add a second field "surName" in two ways

All right, so let's go back to our familiar class example. So let's say we wanted to add a second field. For example, for a surname. So the way we normally do this is we add another key to the state. And we, uh, I'm going to copy and paste this row. It's gonna say surname now. It's going to render a surname, and handleSurnameChange. When I copy and paste this event handler, this will be surname. And I need to bind it. Okay, Mary Poppins. So we can see that it works. 

Uh, so how do we do the same with hooks? So one thing we could do is we could make our state an object. As you can see that the state with hooks state doesn't have to be an object. It can be any primitive. We could make it an object if we wanted to, but we also don't have to. So conceptually surname is, uh, is not closely related to state, uh, to the name. So what we could do is we could declare a second state variable by calling the `useState` hook again. So I'll declare surname. I can give it any name, it's just the variable in my code. And `setSurname`. Calling `useState` and passing the initial state for that state variable 'Poppins'. So again I'm gonna copy and paste the row. Say surname, the value surname, `handleSurnameChange`. And when the user edits the surname, not sir name, we want to set the surname.

Let's see if this works.Yay, it looks like it works. (Applause)

So we can see that we can use hooks more than once in a component. Let's compare the two approaches in more detail. So on the left familiar class component state is always an object, has multiple fields, may call `setState` will merge some something into that object. And then when we want to access it, we do `this.state.something`. On the right in the example using hooks, we use the state hook twice. And that declares two state variables: name and surname. And whenever we call `setName` or `setSurname`, this tells React that it needs to rerender this component, just like if we called setState. And so the next time React renders our components is going to pass the current name and the current surname to our component. And then we can use it directly without accessing `this.state.something`.

### Use React context in two ways

All right. So let's go back to our class example. What else, what other features of React we know?So another thing you might want to do from a component is to read context. So context, in case you're not familiar, it's like, ur, kind of like global variables for a subtree. So it's useful for things like read the current theme like visual theme or the current language that the user is using. And it's useful to avoid passing everything through props if you need all components to be able to read some value. 

So we're going to import ThemeContext and LocaleContext which I already declared in another file. And the API you've probably most familiar with for consuming context, especially if you have to consume multiple contexts, is the render prop API. And it looks like this. So I'm going to
scroll down here. So we can choose ThemeContext Consumer that gives us the theme. In my case, it's just going to be a CSS class. So I copy this, all this code inside the render prop. And I'm going to use `className` equals "theme". All right, very old-timey. (Laughing)

And I also want to show the current language, so I'm going to use LocaleContext Consumer. And it's going to render another row, so I will copy and paste this row, can say language. Language. And render it here. Okay, we can see that context works. 

And that's probably normally consume context. We actually added a more convenient API for accessing it in classes in 16.6. Ur, but this is how you can see multiple contexts. So let's look at the, at how we could do this with hooks. 

So as we said that, state is a fundamental feature of React and this is why you can use state. And so if we want to use context, I need to import my contexts. So this is gonna be a ThemeContext, LocaleContext. And now if I want to use context from my component, I can use context. And then to get the current theme, I can use context ThemeContext. And to get the current locale I can use context LocaleContext. And this doesn't just read the context, it also subscribes the component to updates to this context. But it just gives me the current values, so I can, I can put it into my CSS className. And I can add the bro, that's language, and I can put it here. (Applause)

All right, so let's,let's compare the two approaches. So this is the traditional kind of render prop API. It is very explicit about what it's doing. But it does get a little bit nested and you encounter this not just with context with, with any kind of render prop API. So with hooks, it does the same thing. But it's flat. So we just say we use this context in this context and we get the theme and locale. And then we can use them. So you might be wondering at this point how can React possibly know. For example, I have this two useState calls. So how does it know which state variable corresponds to which useState call. And the answer is that React relies on the order of these calls. This may be a little bit unusual. And, ah, in order for this to work correctly, there is a rule that you need to follow when you use hooks. And the rule is that you cannot call hook inside a condition. It has to be at the top level of your component. So if I do something like, if props condition, and then I call the useState hook here. We actually have a linter plugin that is going to complain that 'This is not the correct way to use hooks'. And we realize that this is an unusual limitation, uh, but it is pretty important for hooks tour correctly and also to enable certain things that I think will you will like that I will show it later.


### How to perform side effects context in two ways

All right, so let's go back to our class. So the other thing you might want to reach for the class for is lifecycle methods. So the most commonly use case for lifecycle methods is you want to perform some side effect such as firing off request, performing some kind of imperative DOM mutation interfacing with the browser APIs. So you might want to do something like this and you can't do during rendering, because it's, it's not rendered yet. So the way you do side effects in React is you declare a lifecycle method like `componentDidMount`. 

And then let's say that, uh, if, let me show this. So you see at the top of the screen, it says 'React App'. So there is actually a browser API that lets us update this. So let's say we want the tab title to be the name of the person and changed it as I type. And so to set it initially I'm going, uh, there is a browser API to do this, is `document.title` equals `this.state.name` plus space plus `this.state.surname`. So now we can see it says 'Mary Poppins'. But then if I, if I edit it, it doesn't get automatically updated, because I also need to implement `componentDidUpdate`. For the, uh, for the side effects to be consistent with what I rendered. So I'm going to declare `componentDidUpdate`, and just copy and paste this. All right, so now says 'Mary Poppins', but if I started editing it, the document title updates. And this is how we perform side effects in a class.

So how do we do this with hooks. Well the ability to perform side effects is another core feature of React components. So if we want to use an effect from our component. Make an import use effect from React. And then we want to tell React what to do after React has flushed our components to the DOM. So we pass a function which is where we perform our effect. So I'm going to say `document.title` equals name plus space plus surname. You can see it says 'Mary Poppins' here. And if I start editing it actually updates. So what default `useEffect` runs both after the initial render and after every update. So by default it is consistent with what here rendered. And you can opt out of this behavior if like for performance reasons and, or if you have special logic. And Ryan's talk after me will touch a little bit on this. 

So let's compare the two approaches. So in the, in the class we divide method ... we divide the logic based on lifecycle method names. So this is why we have `componentDidMount`, `componentDidUpdate`, they fire at different times. And we sometimes repeat some logic between them, we could extract it to a function but still we would have to call it in two places and remember to keep it consistent. 

And with, uh, with the effect hook, the effects are consistent by default although there is a way to opt out of that. And not is that in the class we need to access `this.state`, so there needs to be a special API to do this. But in the effect example, we actually don't need a special API to access the state variable. Because it's already in the scope of the function. It is declared right above. And this is why the effect is declared inside the component rather than the, rather than outside. Because this gives us access to state variables, ability to set them and anything else like the current context value for example, or any of these contexts.


### Subscriptions in two ways

All right, so let's go back to the familiar class example. Ur, another thing you might want to use lifecycle methods for in a class is subscriptions. So maybe you want to subscribe to some kind of browser API and it gives you some value, for example the window size. And you want to update the state in response to changes to this value. And so the way we could do this in a class, let's say that we want to, ur, that we want to monitor the window width. 

So I'm going to put width into state. This window `innerWidth` browser API. And I want to render it. Ur, let me copy and paste this. So this is gonna say width. And I'm going to render it here. It is `this.state.width`. This is the width of the window, not the width of Mary Poppins. (Laughing) And I'm going to add a, ur, I'm going to add an event listener, so we need to actually listen to changes in the width. So at `window.addEventListener`. I'm, I'm going to listen to the resize event, handle resize. And I need to declare this event. And so this is where we're going to update the width state, to be `window.innerWidth`. And we need to bind it. 

And, uh, and I also need to unsubscribe. So I don't want a memory leak with like keeping these subscriptions. I want to unsubscribe from this event. So the way we do this in a class is we create another lifecycle method called `componentWillUnmount`. And I'm going to copy and paste this logic here, except this will `removeEventListener`. So we set up an event listener, and we remove the event listener. And we can verify that this actually works by dragging this. You see the width is changing. So it works. 

So let's see how could ...  how we could do this with hooks. So conceptually listening to the window width has nothing to do with setting the document title. So that's why we're not gonna put it in that effect. It's conceptually completely separate effect and just like we could use state more than once to declare multiple state variables, we can use effect more than once to perform different side effects. 

So I want to subscribe to window `addEventListener`, resize, `handleResize`. And I'm gonna need to keep some state for the current width. So I'm actually going to declare another state variable. So I'll say, ur, width and `setWidth`. We get them by using state with window `innerWidth` as the initial value. And now in my `handleResize` function, I'll just declare it here. Because it isn't used anywhere else. And it's going to `setWidth` to the current width. Um, I need to render it. So I'll copy and paste this row. I'm gonna say width. 

And finally I need to clean up after this effect. So I need to specify how to clean up. And again conceptually cleaning up is part of this effect. So this effect has a clean up place. And the order, you, the way you can specify it is that any effect can optionally return a function. And if it does return the function, then React will call this function to clean up after the effect. So this is where we unsubscribe. Okay, let's just verify that this actually works. Yay. (Applause)

So let's compare the two approaches. On the left, we have a familiar class component, ur, nothing surprising there. We, we have some side effects, some related logic is split apart. So we can see that document title has been set here. But it's also being set here. And then we subscribe to an effect here. Sorry, subscribe to the event here, but we unsubscribe here. So these things need to be in sync with each other. And then this method contains two unrelated methods two unrelated lines. So that me in the in feature make it a bit difficult to test them in isolation. But it looks very familiar. So that's, that's nice.

So this code probably looks less familiar. But let's take another look at what's going on here. Ur, in with hooks we separate code not based on the lifecycle method name, but based on what the code is doing. So we can see that there is one effect which is we updated document title. That's one thing, this component can do. And then there is another effect which is subscribing to the window resize event and update in the state when it changes. And, ur, this effect has a cleanup phase which means that when it's time to remove this effect React removes it and avoids the memory leaks. And if you've been carefully watching, you might notice that since effect run after every render, we're just gonna keep resubscribing. So there is a way to optimize this. Uh, so default is to be consistent, ur, which is important. If you for example use some prop here, I need to resubscribe to a different id from props or something similar. But there is a way to optimize it and opt out of this behavior. And Ryan in the next talk will mention how to do it.

### Custom Hook

All right, so there is one more thing that I want to show here. So this component is getting pretty large, and it's fine. So we expect that since you now can do more in function components, they will get larger and that's totally okay. Um, but you might want to reuse some of that logic in other components or extract it or test it separately. What's interesting though is that hooks calls uh, they are just function calls. And components they are just functions. So how do you share your logic between two functions? You extract it to a different function. That's what I'm going to do. You're going to copy and paste this. And I'm going to create a new function called useWindowWidth and I'll just paste it here. And so we need the width in our component in order to render it. So I need to return it from this function, which is the current width. And then I can go back up. And I can say const `width` equals `useWindowWidth`. (Applause & Cheers)

So what is this function? We didn't do anything special, we just extracted the function. Uh, but there is a convention here. So we are calling this function a custom hook. And by convention custom hook names always start with use. And so there are two reasons for this. 

We're now going to like read your function name or to string it or anything like this. But it is an important convention because first of all, this lets us lint automatically for violation of the first rule that I described about call hooks unconditionally. So if we didn't know if something is a hook, then we wouldn't be able to do that. 

And another reason is that if you just look at the component code, you kind of want to know if some function can have some state inside of it. So it's important that there is a convention is, okay, use something it means that it's potentially stateful. 

And here width gives us the current width and subscribes us to updates through it. So if we wanted to, we could even go further. It's probably not necessary in this example, but I just want to give you like a sense of what you could do. Um, so let's say like maybe setting the document title was a bit more complicated and you wanted to like extract it or test it separately. So I could just copy and paste this. and I could write a new custom hook. I'm gonna call this one `useDocumentTitle`. And so the name and surname don't really make sense in the scope's context. We just want to call this title. And this is going to be an argument, so custom hooks are javascript functions, so they can take arguments and return values or not return. So it is going to take title as an argument. And now in my component, I can say `useDocumentTitle`, `name` plus `surname`.

In fact, I could go even further. So in this case it's totally unnecessary, but again, maybe our inputs were more complicated, maybe we were tracking whether the input was focused and blurred, whether it has been validated, submitted and so on. So maybe we had some more logic there we wanted to pull it out of our components. Um, and reduce duplication. And there is already some duplication, so we have this like almost identical event handlers. 

So what if we could just, um, I'm going to delete one of them and extract the other one. I'm going to create a new hook, that I'm going to call `useFormInput`. So this is my change handler. Now I'll also copy and paste this declaration. So this defines the state for this input. And so it's no longer `name` and `setName`. I'll just call generically `value` and `setValue`. It's going to take the initial value as an argument. And this is just going to be a `handleChange`, and this will `setValue`. So what do we want to get in order to use this, uh, use an input in our component? We want to get the current value, and a change handler. These are the things that we attach to the input. So let's just return them. Um, return `value` and `onChange` `handleChange`. So now if we go back up, we can say name equals `useFormInput` Mary. The name is going to be an object with `value` and `onChange` fields. And `surname` is `useFormInput` Poppins. So this is now going to be `named.value` and `surname.value` because this is where the string is. And so now I can remove this, and I can spread over the name object. Someone is laughing. (Laughing) All right. Let's just verify it and break it, yeah it works. 

So each time we call a hook, its state is completely isolated. And this is because we just rely on the order of hook calls, and not on names or anything. So you can call the same hook multiple times. Each call will get its own local state.


So let's compare the two approaches for the last time. Um, so on the left we have a familiar class component, it has some, some state in an object, bind some methods, has some logic spread across different lifecycle methods as a bunch of event handlers, um, uses, um, uses something things from the context and render stuff. Um, pretty familiar. 

And on the right pane, this may not look like the React components were used to. But it kind of makes sense. Even if you don't know how these functions are implemented. You can see okay it uses to form inputs,  uses some context to get theme and locale, it uses the window width and document title, and it renders a bunch of stuff. And if we want to. We can scroll further and we can see okay, so this is how the input works, this is how setting the document title works, this is how the window width subscription works. Or maybe this could be an npm package and you don't actually need to know that. All we could pull it back into a component or copy and paste between components. So hooks give you custom hooks, give you the flexibility to create your own abstractions that are not ...  they do not inflate your React component tree and avoid the wrapper hell. (Applause)

And importantly these are not two separate applications. So this is actually one application. So I have this window open just to demonstrate that classes can work side by side with hooks. And while hooks represent our vision for the future of React. Uh, we don't want to make breaking changes like this. So we need to keep classes working.

## Hooks Proposal
Ur, let's go back to you the slides. All right, now this is a slider you can actually tweet. (Laughing)

We present the Hooks proposal to you today. Uh, hooks let us to use all React features without having to write a class. They do not deprecated classes, but you have the option to not have to write them. We intend to cover all use cases for classes with hooks as soon as possible. There are a few that are missing, but we're working on them. And hooks let you reuse stateful logic, extracted out of components, tested separately, reuse it between different components without introducing the wrapper hell. 

And again importantly it's not a breaking change, completely backwards compatible, strictly addition ..., uh, additive. And you can find the, we wrote the documentation for hooks, so you can find it at this url. Um, and we want to hear from you, the React community wanna hear what you think about hooks. Um, whether you like them or not. And we realize that it's pretty hard to give feedback without actually trying them. So we built them, and we released sixty ..., um, sixteen seven alpha. It's not a major release, it's a minor release. But in alpha where you can try hooks. And we've been trying them in production at Facebook for about a month. So we don't expect major bugs there. But the APIs themselves may change together with your feedback. And I asked you not to rewrite anything like not to rewrite their whole apps with hooks. Because first of all it's, it's a proposal. And second because personally I, I find that it takes a bit of a mind shift to start thinking in hooks, and it might be a bit confusing if you try to just take a class component and convert it. But I do encourage you to try using hooks in some of the newer code that you write in and let us know what you think. So, thank you. (Applause)

So in our view hooks represent our vision for the future of React. But I think they also represent the way we move React forward. And that is we don't do big rewrite. Uh, we want the new patterns that we like better to coexist with the old patterns, so that we can have gradual migration and adoption just like you can gradually adopt React itself. 

## Dan's personal note

And this is almost the end of my talk. Uh, I want to end it on a personal note. So I started learning React about four years ago. And one of my first questions was why JSX. 

Uh, but my second one of the next questions was I can figure out what does the logo have to do with React. So the project is not called Atom, it's not a physics engine. Um, so one interpretation is that it's kind of upon on reactions, so atoms participate in chemical reactions, reactions -> React. 

Um, but it's not a claim with React actually got it, uh, I found a different interpretation that made more sense to me. And the way I think about it, uh, we know that physical matter consists of atoms. And we've learned that it's the types of these atoms and their properties that determine how the physical matter looks and behaves. And React has taught me something similar, that you can take a user interface and you can split it into these independent units called components, and it's the types and properties of these components that can describe how the user interface looks and behaves. 

What's ironic though is that the word 'atom', it literally means indivisible. So when scientists just discovered atom for the first time, they thought this is the smallest thing we're gonna find. But later they discovered an electron, which is a smaller particle inside the atom. And it turned out it actually electrons explain a lot about how atoms work. 

And I kind of feel the same way about hooks. I don't feel like hooks are a new feature. Rather I feel that hooks provide me with access to React features that I already know, such as state and context and lifecycle. And I feel like hooks are a more direct representation of React. And that they really explain how a component works inside. And I feel like they've been hiding in plain sight for four years. And in fact if you look at the React logo, you can see those electron orbits there. So maybe hooks have been there all along. Thank you. (Applause)