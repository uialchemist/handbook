# Day-to-day workflows

This article explains the preferred practices and day-to-day of being a Florence developer. Read through it at least once to understand how we work and why.

### 1. 'MVP:Extend' Methodology

Florence operates under a startup methodology we call *MVP:Extend*, which in praxis means to implement any feature or issue with the *least possible time spent* doing so. This way we can launch features and fixes early and clean up later, if needed. The point of MVP:Extend is to see if anyone even uses the product or feature before investing too much time into making it great.

Here's an example of MVP:Extend

> **Feature Request**  
> We want a help centre to better explain to users how the platform works, and to reduce customer service load. It should have search, categories, video posts, the ability to upvote/downvote and a CMS functionality so the team can add and edit articles.
>   
> **MVP**  
> Let's look at what is actually necessary to launch this feature. All we need is for a user to find help on a specific topic. So for now we can easily remove categories, up/down voting, and video posts. We need the CMS portion so we can manage the articles, but it doesn't need to look good as it's only used for internal purposes. So the MVP of this feature could just be articles, search and a dirty CMS.
>   
> **Extend**  
> After we launch the MVP we take a look at how it performs and how it is being used. It turns out there are many articles, so categories makes sense. Also turns out we have plenty of video content so we should have special video articles. Up/down voting is handy because it gives the internal team an indication of article quality. And the CMS portion could do with some love.

### 2. From Feature Idea To Implementation & Launch

1. Small product meeting to discuss the micro decisions
2. Figure out the MVP:Extend priority
3. Create GitHub issues to split the feature into digestible parts
4. Create new branch
5. Prototype in HTML/CSS using either dummy or live data
6. Small product meeting to figure out if any changes are needed
7. When done, submit pull request
8. Launch MVP set

### 3. Day-To-Day Workflow
These points only apply to our generalist developer positions. Specialised roles follow separate workflows.

0. Every Monday at 11am we have our weekly tech meeting to figure out the main priorities for the week.
1. We use a weekly canban board using GitHub projects. Grab the top issue assigned to you in the 'To do' column, create a new branch, create a pull request for that branch and then make sure the PR is assigned to you and belongs to the current project.
2. If you're on bug duty, Sentry is your top priority at all times. Make sure your Sentry notification settings are on.
3. If there are open PRs, feel free to jump in on any of them at any time and add review comments or questions if you have no other pressing issues to deal with.
4. Eat snacks.
5. Friday Funtime™ is designated time to do something different to the rest of the week. It can be working together on a new feature and try to create and launch it in one day, or it could be completely unrelated to Florence.

### 4. HTML/CSS

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

### 5. Refactoring

If you need a break from a problem or normal day-to-day routines, feel free to spend time refactoring code into more elegant and/or performant solutions. We tend to use the excellent Ruby guides written by Deliveroo as a loose guide when we refactor: https://deliveroo.engineering/guidelines/

### 6. Performance

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
