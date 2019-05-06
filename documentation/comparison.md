# Comparison with other Libraries

This section cannot escape some bias but I think it is important to understand where Solid's solution sits compared to other libraries. This is not about performance. For a definitive look at performance feel free to look at the [JS Framework Benchmark](https://github.com/krausest/js-framework-benchmark).

## Knockout.js

This library owes its existence to Knockout. Modernizing its model for fine grained dependency detection was the motivation for this project. Knockout was released in 2010 and supports back to IE6 while much of Solid doesn't support IE at all(although you can use Signals in Solid without using the Proxy State object).

Knockout's bindings are just strings in HTML which are walked over at runtime. They depend on cloning context($parent etc...). Whereas Solid uses JSX or Tagged Template Literals for templating opting for an in JavaScript API.

The biggest difference might be that Solid is built on S.js which has explicit mechanisms to handle batching of changes whereas Knockout has deferUpdates which uses a microtask queue, and S has a hierarchal model to automatically handle garbage cleanup and Knockout only has explicit disposal built in.

## React

React is also has had a big influence on Solid. its unidirectional flow and explicit segregation of read and write in its Hooks API informed Solid's API. More so the objective of being just a "Render Library" rather than a framework. Solid has strong opinions on how to approach managing data in application development but doesn't seek to constrain its execution.

However, as much as Solid aligns with React's design philosophy, it works significantly different fundamentally. React uses a Virtual DOM, and Solid does not. React's abstraction is top down component partition where render methods are called repeatedly and diffed. Whereas Solid renders each Template once in entirety constructing its reactive graph and afterwords only executes instructions related to fine-grained changes.

## Svelte

Svelte pioneered the precompiled disappearing framework that Solid also employs to a certain degree. Both libraries are truly reactive and can produce really small execution code bundles although Svelte is the winner there. Solid requires a bit more explicitness in its declarations and relying less on implicit analysis from the compiler, but that is part of what gives Solid superior performance.

Both libraries aim to help their developers write less code but approach it completely differently. Svelte 3 focuses on the optimization of the ease of dealing with localized change focusing on plain object interaction and 2 way binding in constrast to Solid's deliberate choice to embrace CQRS and immutable interface. Solid uses proxies to track dependencies but very consciously blocks setters, considering 2 way binding and direct mutation a dangerous anti-pattern in terms of large scale solutions. Instead Solid has adopted expressive setters(influenced by ImmutableJS and Falcor) to give flexibility of plain objects, with greater brevity, and significantly more control.

Svelte still represents pushing the boundaries of precompilation where Solid is a bit more conservative. But we feel Solid really takes the best of both worlds.

## LitHTML & LighterHTML

These libraries are incredibly similar and have had some influence on Solid. Mostly that Solid's compiled code uses a very similar method to performantly initially render the DOM. Cloning Template elements and using comment placeholders are something that Solid and these libraries share in common.

The biggest difference is that while these libraries do not use the Virtual DOM they treat rendering the same way, top down and requiring component partitioning to keep things sane. By contrast Solid uses its fine grained Reactive Graph to only update what has changed and in so only shares this technique for its initial render. This allows for it benefit from the initial speed only available from native DOM and also have the most performant approach to updates.

## Vue

Solid is not particularly influenced by Vue, but they are relatable. They both use Proxies over a Reactive system, but that is where the similarities end. Vue's fine grained dependency detection just feeds into a less fine-grained Virtual DOM and Component system whereas Solid keeps its granularity right down to its direct DOM updates.

Vue works off configuration objects where Solid uses a more functional approach instead opting for composeable primitives. Vue credits its configuration as an easy learning curve, but we feel having fewer simple primitives actually reduces mental overhead in a similar way. Where configuration objects or lifecycles require learning and remember how they apply(like a checklist), Solid's primitives are not unlike understanding what Array.map does. Once you have the tool you can do the job.

Vue also is setup for direct mutation and 2 way binding, which helps keep the code small. However, Solid's functional approach often leads to less setup than Vue's configuration objects.

## RxJS

RxJS is the Reactive library. While Solid has a similar idea of Observable data it uses a much different application of the observer pattern. While Signals are like a simple version of an Observable (only the next), the pattern of auto dependency detection surplant's RxJS' hundred or so operators. Solid could be handled this way and earlier versions of the library included similar operators, but in most cases it is more straightforward to write your own transformation logic in a computation. Where Observables are cold starting, unicast, and push-based many problems on the client lend to hot startup and being multicast which is Solid's default behavior.

## Others

Angular and a few other popular libraries are notably missing from this comparison and it is mostly lack of experience with them to give an adequate comparison. Mostly larger Frameworks do not share much in common with Solid and it is much harder to compare them headon.
