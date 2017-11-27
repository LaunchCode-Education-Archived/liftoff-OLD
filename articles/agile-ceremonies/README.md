---
title: 'Agile Ceremonies'
currentMenu: 'articles'
---

[Agile ceremonies](https://www.atlassian.com/agile/ceremonies) are types of team communications (i.e. meetings) structured in specific ways meant to enable quick, iterative, efficient, and communicative development cycles. We'll use a few of them in this course.

- [Sprint Kickoff](#sprint-kickoff)
    - [Prioritization](#prioritization)
    - [Estimation](#estimation)
- [Standup](#standup)
- [Retrospective](#retrospective)

## Sprint Kickoff

A **sprint** is a time-limited work cycle during which teams focus on delivering as much value as possible. Sprints can have different lengths, but most teams will use 1 or 2-week sprints. We'll go with 2-week sprints in this course. This means that when we're planning the work for a sprint, you should think about how much work you can do in a 2-week timeframe.

### Prioritization

Decide which user stories are the most important for you to work on next. Take into consideration any dependencies between stories, while also trying to keep stories as small as possible. This is a great time to clarify and further define user stories. You'll often break up or combine stories during a kickoff meeting.

### Estimation

Once you've prioritized your stories, you'll need to estimate them in order to get an idea of how much work you'll be able to do during a sprint. When estimating, you'll often need to clarify details with your mentor, or with the "product owner" (which in this case is you). Make sure you have a clear understanding of what needs to be done, and the scope of work involved. 

Using Pivotal Tracker, user stories are estimated in "points", which are an intentionally subjective unit. After a few sprints, you'll get a good hang of estimation, but the first few times you'll like over or under-estimate. That's okay! Take note of what you did well and what you could do better during the estimation process and apply it next time.

Here's a rough estimation rubric for using the 0,1,2,3 (Linear) point scale of Pivotal Tracker:

**0 points:** Zero-point changes are almost always bug fixes. In the world of agile development, work that doesn't add explicit additional value to a product is not usually not counted or measured. You might also add something inconsequentially small in this category, such as a copy/text change.

**1 point:** A small change that, similar to things you have done before and are confident you can do again without having to learn anythign new. For example:
- Adding an input to a form
- Adding a field to a model class
- Adding a static or infomrational page

**2 points:** A medium-sized change that may involve some components that are new to you, or take slightly more time. For example:
- Enable pagination for a listing of blog posts
- Adding a simple form, such as one that creates a new isntance of a model class (e.g. a blog post)

**3 points:** A large task with several components. These often entail changes to several views and controllers, or learning something significant and new. For example:
- Adding a new model class with one-to-many or many-to-many relationships to other classes
- Adding a complicated form, such as registration or login

<aside class="aside-pro-tip" markdown="1">
If you estimate a story at 3 points, make sure that you can't break it down into multiple smaller stories.
</aside>

After you have estimated the most important stories, move those that you want to complete into your *Current Iteration* panel in Pivotal Tracker. All other stories should be in the *Backlog* (for those you want to work on soon, but not during this sprint), or in the *Icebox* (for things that you may work on in the future, but are not important in the short-term).

**Be frank and honest about how much you work you can do in a sprint!** You'll likely soon learn one of two things: you tend to overestimate work, or you tend to underestimate. Take this self-knowledge to heart, and use it to make yourself a better planner.

The amount of points you complete in a given sprint contributes to your **velocity**. Loosely speaking, velocity is the average number points that you are able to complete during a sprint, averaged over some number of past sprints. Knowing your velocity can also help you estimate work going forward.

## Standup

A **standup** is a short status meeting conducted regularly (usually every morning) by agile teams. In it's most common format, standups take place by having the entire team stand up near their work area and take turns answering the following questions:

- What did I complete yesterday?
- What will I work on today?
- Am I blocked by anything?

By "blocked" it is meant that you are prevented from working on or completing task due to a circumstance outside of your direct control. You could be waiting for another team member or an external party to deliver something necessary for you to proceed.

Standups have a few purposes.

**Provide a quick, regular structure for communication**

When teams need to move quickly, regular and adaptive communication is essential. You shouldn't have to wait for a weekly team meeting, or some other time, to understand what your fellow developers are working on. Maybe you can provide insight on something they're working on, or maybe you're blocking them from moving forward and weren't aware.

**Encourage collaborative accountability**

When everyone has to report on what they worked on yesterday, nobody wants to be the person that didn't make any progress. Sure, some days will be more productive than others, but it's important that each team member put their best effort forward to enable the most postive outcome for the team overall.

**Brevity and participation**

This is why we stand up! Keeping the meeting quick and informal is paramount for optimal efficiency. When teams sit down in a conference room, the tendency is for long, drawn-out meetings where not everyone is equally engaged to take place.

In the context of Liftoff, you will conduct regular standups within your team. While you aren't all working on the same project, you should cover each of the three items above. Here's an adapted rubric to use when it's your turn to report:

- What did I complete last week?
- What will I work on next week?
- Am I blocked by anything? In other words, is there anythign a mentor or fellow learner could help me understand to move forward more quickly?

## Retrospective

**Coming soon...**