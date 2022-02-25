# Rails Best Practices
## By Andy Maleh (Two-time RailsConf Speaker, RubyConf/MagicRuby/MountainWest RubyConf Speaker, and Fukuoka Ruby 2022 Special Award Winner)

1. [Smalltalk-Style MVC with smart Views pulling data instead of dumb Views with Controllers pushing data](https://andymaleh.blogspot.com/2011/10/decoupling-views-from-controllers-in.html)
1. [Rails Helper Presenters of Model information](https://andymaleh.blogspot.com/2011/10/decoupling-views-from-controllers-in.html)
1. No need for "View Component" libraries as Rails already supports View Components via Rails Partials and well-named Rails Helper methods. It is recommended that Partial View components accept both options of instance variables and local parameters to simplify the code further in the case of a controller action setting instance variables (by not passing any local parameters to Rails Partials in that case).
1. Modals are displayed instantly without making web requests (to avoid hindering user experience) by including as hidden Rails Partials in web pages that need them
1. Use [State Design Pattern](https://en.wikipedia.org/wiki/State_pattern), which keeps the code close to the business domain, instead of State Machine (which is a low level computer science concept that distances the code from the business domain, thus not recommended except in the theoretical computing field)
1. If all web application pages have less than 500 DOM elements (the majority of web applications), avoid all JavaScript SPA frameworks at all costs as they multiply the cost of code maintenance by almost 100x times! Use "JavaScript Sprinkles" sparingly instead, using [jQuery](https://jquery.com/) (avoid Stimulus et al because they include some redundancy and over-engineering in the required element IDs/Classes/Controllers that could be avoided in [jQuery](https://jquery.com/)). Also, [jQuery](https://jquery.com/) is still better, simpler, and more convenient than all SPA frameworks, and even vanilla JavaScript despite all the ES improvements, as [jQuery](https://jquery.com/) results in the tersest code possible.
1. Implement Wizards (Multi-Step Forms) as simply nested views of a resource's Edit/Update operation as per the [Ultra Light Wizard Architectural Pattern](https://github.com/AndyObtiva/ultra_light_wizard), which is supported by the [Wicked](https://github.com/zombocom/wicked) gem.
1. Avoid repeated conditionals by refactoring to [Strategy Design Pattern](https://en.wikipedia.org/wiki/Strategy_pattern) (or in some cases [State Design Pattern](https://en.wikipedia.org/wiki/State_pattern)), manually or using gems like [Strategic](https://github.com/AndyObtiva/strategic).
1. Avoid using Dependency Injection containers as they are not necessary in Ruby due to its highly malleable dynamically typed nature. Dependency Injeciton containers come from Statically Typed languages like Java, and do not truly belong in Ruby as they are considered extreme over-engineering in it that multiplies code maintenance cost through unnecessary indirection.
1. Avoid all misguided programming techniques that impractically require immutability, over-emphasize mathematical functional computations, and neglect natural Object-Oriented abstractions (e.g. having an Order object represent a real world Order that encapsulates all the included products and quantities) as they result in code that is distanced from the business domain model (thus losing half the battle already in meeting the demands of customers intuitively) and is very difficult to maintain except by the one overly-proud self-congratulatory person who wrote it (who won't be able to maintain it once some time has passed too as they hop unto the next buzzword hyped misguided programming technique to rewrite the bad code they could not comprehend anymore).
1. Use the right tool for the job! Avoid any sort of Static Typing in Rails application code whether using TypeScript, Elm, PureScript, or any of the Ruby static typing libraries like Sorbet and RBS. Ruby and JavaScript were never meant as statically typed languages, and there are direct benefits to be had in their dynamically typed nature that must not be forgone when building Ruby on Rails apps. People who use static typing in Ruby or JavaScript are like someone who is handed a fast bike, but decides to install 2 third-wheels and ride it like a kid's tricycle very slowly. If you need static typing for few scenarios or parts of the application that have very high performance requirements, learn to be a Polyglot instead, and use Ruby in tandem with another statically typed programming language like Java (especially with JRuby), C, C++, Crystal, or Swift. Finally, if you are not seeking performance yet correctness through static typing, then write automated tests instead since you need them anyways whether the language is static or dynamic.
1. Cover sensitive parts of your Ruby on Rails app with automated tests. But, avoid silly policies mandating 100% code coverage or 100% TDD. Remember that serving the customer is always the highest goal, and sometimes that is done by not wasting time on unnecessary tests (like when quickly implementing a CRM through a Rails Engine). 
