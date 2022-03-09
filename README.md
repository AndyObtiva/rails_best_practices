# Andy's Rails Best Practices

**Authored by Andy Maleh (Two-time RailsConf Speaker, RubyConf/MagicRuby/MountainWest RubyConf Speaker, and Fukuoka Ruby 2022 Special Award Winner)**

## Software Architecture:

1. [Smalltalk-Style MVC with smart Views pulling data instead of dumb Views with Controllers pushing data](https://andymaleh.blogspot.com/2011/10/decoupling-views-from-controllers-in.html). In other words, avoid having Controllers that push tons of variables to the View with `before_action` hooks (even if all those hooks were extracted to modules) as that results in very inflexible unmaintainable code that does not scale well as the application grows bigger. It is much simpler to define methods that pull the variables (instead of pushing it) when necessary only, which are reusable everywhere instead of just in the controllers that push the variables, thus saving the developer from doing `before_action` duplications across Controllers or creating unnecessary deep inheritance Controller hierarchies.
1. If web application pages never spawn more than 20,000 DOM elements all at once in JavaScript (99% of web applications) and have very few or no client-side-only interactions, avoid all JavaScript SPA frameworks at all costs as they do not make any performance difference (less than 20,000 DOM element updates occur in less than 500ms, thus appearing instantaneous to users) or productivity difference and they multiply the cost of code maintenance by almost 100x times! Use "JavaScript Sprinkles" sparingly instead, using [jQuery](https://jquery.com/) (avoid Stimulus et al because they include some redundancy, wheel-reinvention, and over-engineering in the required element IDs/Classes/Controllers that could be avoided in [jQuery](https://jquery.com/)). Also, in the aforementioned common cases, [jQuery](https://jquery.com/) is still better, simpler, and more convenient than all JavaScript SPA frameworks, and even vanilla JavaScript despite all the ES improvements, as [jQuery](https://jquery.com/) results in the tersest code possible and only applies updates surgically (the server generates all elements), so performance is very fast with it.
1. In the case of web applications with pages that need to spawn/update more than 20,000 DOM elements all at once on the frontend (like rare SVG heavy applications) or applications that have a lot of client-side interactions that do not require the server, use [Opal Ruby](https://github.com/opal/opal-rails) instead of JavaScript (potentially with an [Opal framework](https://github.com/fazibear/awesome-opal) or just [Opal-JQuery](https://github.com/opal/opal-jquery/), encapsulating any JavaScript logic that is performance-sensitive if needed). Ruby is after all much more readable, maintainable, and productive than all ES6+ incarnations and frameworks combined. And, you can build all your frontend abstractions (e.g. EmailFolder, EmailMessage, Contact, etc..) in pure Ruby, thus have frontend code that is as maintainable as server-side Rails code.
1. Implement Wizards (Multi-Step Forms) as simply nested views of a resource's Edit/Update operation as per the [Ultra Light Wizard Architectural Pattern](https://github.com/AndyObtiva/ultra_light_wizard), which is supported by the [Wicked](https://github.com/zombocom/wicked) gem.
1. Avoid using Dependency Injection containers as they are not necessary in Ruby due to its highly malleable dynamically typed nature. Dependency Injeciton containers come from Statically Typed languages like Java, and do not truly belong in Ruby as they are considered extreme over-engineering in it that multiplies code maintenance cost through unnecessary indirection.
1. Use the right tool for the job! Avoid any sort of Static Typing in Rails application code whether using TypeScript, Elm, PureScript, or any of the Ruby static typing libraries like Sorbet and RBS. Ruby and JavaScript were never meant as statically typed languages, and there are direct benefits to be had in their dynamically typed nature that must not be forgone when building Ruby on Rails apps. People who use static typing in Ruby or JavaScript are like someone who is handed a fast bike, but decides to install 2 third-wheels and ride it like a kid's tricycle very slowly. If you need static typing for few scenarios or parts of the application that have very high performance requirements, learn to be a Polyglot instead, and use Ruby in tandem with another statically typed programming language like Java (especially with JRuby), C, C++, Crystal, or Swift. Finally, if you are not seeking performance yet correctness through static typing, then write automated tests instead since you need them anyways whether the language is static or dynamic.
1. Consider using [Rails Engines](https://guides.rubyonrails.org/engines.html) and [Rails Engine Patterns](https://www.slideshare.net/AndyMaleh/rails-engine-patterns) when you have components/concepts/objects that are repeated in multiple Rails applications, like in the case of multiple Rails applications sharing the same core domain model logic.  
1. Only build Rails API web services in the cases of needing to modularize and expose some of your domain models publicly to B2B or B2C clients or support a SPAs (single page applications). Favor Rails Engines and avoid building Rails API web services (incorrectly named microservices by misguided programmers) in the situations of needing private reuse of domain models across Rails applications given that services increase complexity of setup/maintenance, latency, and security risks of web applications.
1. Use CDNs for static assets in your Rails application to offload serving them from your web application, thus conserving your server CPU/Memory resources and relying on delivery-optimized CDNs, which serve static assets faster through location replication.
1. Setup a performance monitoring service like New Relic when building an application that needs to serve many customers in order to recognize the weakest links in your application and optimize them.
1. Add database indices to all columns that are used frequently in database queries by customers, but only once you confirm that the table has a very large number of rows and performance is not fast enough without the indices given there are trade-offs in adding them. Avoid adding too many indices or any to database tables that undergo very frequent inserts/updates (much more than reads). 
1. In some cases, you might need to denormalize/replicate database tables to optimize performance for both reads (e.g. reports) and writes (e.g. transactions).
1. Use [Rails caching](https://guides.rubyonrails.org/caching_with_rails.html) at every level it is beneficial, but only when needed (web requests are taking more than 500ms to process) as caching complicates code maintainability.
1. You neither want fat-model-skinny-controller nor skinny-model-fat-controller. Always aim at skinny-model-skinny-controller-skinny-view (yes, skinny everything, including the view too). What does that practically mean? Always refactor your code so that your classes/templates do not cross 200 lines of code. If a file grows too big whether a controller, a model, or a view, then divide and conquer it with understandable business domain model concepts (or Rails Partials/Helpers in the case of views), potentially following [GoF Design Patterns](https://en.wikipedia.org/wiki/Design_Patterns) and [Domain Driven Design Patterns](https://en.wikipedia.org/wiki/Domain-driven_design). You can sometimes relax the 200-lines-of-code restriction a bit, but certainly not more than 500 lines and if you have a file with 1000 lines, you're clearly in the unmaintainable code danger zone.

## Software Design:

1. [Rails Helper Presenters of Model information](https://andymaleh.blogspot.com/2011/10/decoupling-views-from-controllers-in.html)
1. No need for "View Component" libraries as Rails already supports View Components via Rails Partials and well-named Rails Helper methods (remember that less is more with software engineering, so no need to re-invent the wheel with unnecessary view component libraries).
1. Modals (aka dialogs) are displayed instantly without making web requests (to avoid hindering user experience) by including as hidden Rails Partials in web pages that need them and then activating from front-end code when needed.
1. Avoid repeated conditionals by refactoring to the [Strategy Design Pattern](https://en.wikipedia.org/wiki/Strategy_pattern) (or in some cases [State Design Pattern](https://en.wikipedia.org/wiki/State_pattern)), manually or using gems like [Strategic](https://github.com/AndyObtiva/strategic).
1. Use the [State Design Pattern](https://en.wikipedia.org/wiki/State_pattern) when having state-dependent logic, keeping the code close to the business domain, instead of the State Machine approach, which is a low level Computer Science concept that distances the code from the business domain, thus not recommended except in the theoretical computing field.
1. Avoid all misguided programming techniques that impractically require immutability, over-emphasize mathematical functional computations, and neglect natural Object-Oriented abstractions (e.g. having an Order object represent a real world Order that encapsulates all the included products and quantities) as they result in code that is distanced from the business domain model (thus losing half the battle already in meeting the demands of customers intuitively) and is very difficult to maintain except by the one overly-proud self-congratulatory person who wrote it (who won't be able to maintain it once some time has passed too as they hop unto the next buzzword hyped misguided programming technique to rewrite the bad code they could not comprehend anymore).
1. Cover sensitive parts of your Ruby on Rails app with automated tests. But, avoid silly policies mandating 100% code coverage or 100% TDD. Remember that serving the customer is always the highest goal, and sometimes that is done by not wasting time on unnecessary tests (like when quickly implementing a CRM through a Rails Engine). 
1. Apply the [Value Object](https://www.domainlanguage.com/wp-content/uploads/2016/05/DDD_Reference_2015-03.pdf) pattern when objects are identified by their attributes without ever needing an update (e.g. a zip code object identified as 60611). It simplifies the code, avoids the need for a disk-based database table or joins thus improving querying performance around those objects, and enables simpler caching techniques.
1. Use [ActiveModel](https://guides.rubyonrails.org/active_model_basics.html) to model objects that do not require persistence instead of ActiveRecord. Some objects represented by Rails resources do not truly need ActiveRecord since they are transient, meaning used temporarily in a transaction to do things like send an email or perhaps produce multiple ActiveRecord models that are stored in the database. Avoid any libraries that provide typed struct support as they are not Ruby-idiomatic and if that is ever needed (for performance optimization only), you could use a statically typed language that is more suitable for that need. 
1. [Divide and conquer your Rails Routes when they get out of hand](https://andymaleh.blogspot.com/2013/04/managing-rails-routes-when-they-get-out.html)!
1. Avoid overuse of [Active Record Callbacks](https://guides.rubyonrails.org/active_record_callbacks.html) by dividing and extracting their logic into meaningful Observers and mixin modules instead, considering the [Wisper](https://github.com/krisleech/wisper) gem.
1. Avoid fashionable "monad" libraries! They are inferior to Ruby-idiomatic techniques!

**Bad example using dry-monad:**

```ruby
def find_user(user_id)
  user = User.find_by(id: user_id)

  if user
    Success(user)
  else
    Failure(:user_not_found)
  end
end

def find_address(address_id)
  address = Address.find_by(id: address_id)

  if address
    Success(address)
  else
    Failure(:address_not_found)
  end
end

user = yield find_user(params[:user_id])
address = yield find_address(params[:address_id])
Maybe(user.update(address_id: address.id))
```

**Good example re-written using Ruby-idiomatic techniques:**

```ruby
user = User.find_by(id: user_id)
address = Address.find_by(id: address_id)
address && user&.update(address_id: address.id)
```

See, how it is much shorter and simpler, let alone it does not require codebase newcomers to learn a new library that is unnecessary! 

---

Hit me up in [Issues](https://github.com/AndyObtiva/rails_best_practices/issues) if you have any questions about best practices you do not understand or think there are mistakes in the best practices.

Feel free to submit [Pull Requests](https://github.com/AndyObtiva/rails_best_practices/pulls) if you know of best practices that are missing and would like to add to this best practice list.
