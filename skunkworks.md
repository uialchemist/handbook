# Skunkworks

This article explains the preferred practices and day-to-day of the Skunkworks developer team at Florence. Read through it at least once to understand how we work and why.

### 1. 'MVP:Extend' Methodology

Florence operates under our own startup methodology we call *MVP:Extend*, which in praxis means to implement any feature or issue with the *least possible time spent* doing so. This way we can launch features and fixes early and clean up later, if needed. The point of MVP:Extend is to see if anyone even uses the product or feature before investing too much time into making it a minimum _lovable_ product.

Here's an example of how we use MVP:Extend

> **Feature Request**  
> We want a help centre to better explain to users how the platform works, and to reduce customer service load. It should have search, categories, video posts, the ability to upvote/downvote and a CMS functionality so the team can add and edit articles.
>   
> **MVP**  being
> Let's look at what is actually necessary to launch this feature. All we need is for a user to find help on a specific topic. So for now we can easily remove categories, up/down voting, and video posts. We need the CMS portion so we can manage the articles, but it doesn't need to look good as it's only used for internal purposes. So the MVP of this feature could just be articles, search and a dirty CMS.
>   
> **Extend**  
> After we launch the MVP we take a look at how it performs and how it is being used. It turns out there are many articles, so categories makes sense. Also turns out we have plenty of video content so we should have special video articles. Up/down voting is handy because it gives the internal team an indication of article quality. And the CMS portion could do with some love.

### 2. From Feature Idea To Implementation & Launch

0. Discovery phase to figure out the problems, brainstorm solutions, and decide on measurable targets
1. Figure out the MVP:Extend groupings
2. Solutions are turned into GitHub issues for MVP phase
3. UI prototype created for UX testing and signoff
4. Wire everything up and launch MVP
5. Monitor feature to see if it's being used and/or hitting its targets
6. Decide on the Extend set (if needed) and repeat steps 3-6

### 3. HTML/CSS

We use pure SASS (not SCSS), and a mix between Object-Oriented CSS (OOCSS) and Block/Element/Modifier (BEM) in conjunction with HAML as the HTML templating language. Here is an example of how we mix the two:

```haml
.content
  .content-inner.site-width
    .content-inner--header
      %h1 Hello, world
    .content-inner--action
      %a.button.button-blue{href: "#"} Hello
```
```sass
.content
  width: 100%

.content-inner--header
  margin: 20px 0

.button
  padding: 20px
  background: $dark

.button-blue
  background: $blue

.site-width
  width: 1140px
```

### 4. Refactoring

If you need a break from a problem or normal day-to-day routines, feel free to spend time refactoring code into more elegant and/or performant solutions. We tend to use the excellent Ruby guides written by Deliveroo as a loose guide when we refactor: https://deliveroo.engineering/guidelines/

### 5. Performance

For anything under the banner of MVP, quick hacks like `.select` and `.count` is fine (not really), but always try to aim to do things performantly from the start. Can that `.select` be made into a `.where` instead? Are you using `.count` instead of the (in most cases) more performant `.size`? Are you using `.size > 0` instead of `.any?`, and so on.

Classic and quick example of lazy vs correct:

```ruby
- if User.select{|u| u.timesheets.where(paid: true)}.count > 0
  "You have paid timesheets!"
- else
  "You don't have any paid timesheets."
```

Golly. This should really be cleaned up into something like:

```ruby
= User.all.joins(:timesheet).where('timesheets.paid = ?', true).any? ? "You have paid timesheets!" : "You don't have any paid timesheets."
```

Cleaner and more performant.
