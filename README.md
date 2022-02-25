# Rails Best Practices

- [Smalltalk-Style MVC with smart Views pulling data instead of dumb Views with Controllers pushing data](https://andymaleh.blogspot.com/2011/10/decoupling-views-from-controllers-in.html)
- [Rails Helper Presenters of Model information](https://andymaleh.blogspot.com/2011/10/decoupling-views-from-controllers-in.html)
- Rails Partial View components that accept either instance variables or local parameters
- Modals are displayed instantly without making web requests (to avoid hindering user experience) by including as partials in web pages that need them
- Using [State Design Pattern](https://en.wikipedia.org/wiki/State_pattern), which keeps the code close to the business domain, instead of State Machine (which is a low level computer science concept that distances the code from the business domain, thus not recommended except in the theoretical computing field)
- If application webpages have less than 500 DOM elements (the majority of web applications), avoid all SPA frameworks at all costs as they multiply the cost of code maintenance by almost 100x times! Use "JavaScript Sprinkles" sparingly instead, using jQuery (avoid Stimulus et al because they include some redundancy and over-engineering in the required element IDs/Classes/Controllers that could be avoided in jQuery). Also, jQuery is still better, simpler, and more convenient than all SPA frameworks, and even vanilla JavaScript despite all the ES improvements, as jQuery results in the tersest code possible.
