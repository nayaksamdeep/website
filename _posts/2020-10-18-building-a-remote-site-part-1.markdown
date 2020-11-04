---
layout: post
title:  "Building an Org at a Remote Site - Part 1"
date:   2020-10-18 00:36:19 +0530
categories: remote
---
An organization is a set of people working towards a common goal. An
organisation can represent an entire company or can represent a smaller
subset of the company who are tasked with a common goal. While building
an Org is applicable to all domains, in this blog series I will focus on
sharing my experience with software companies that follow agile
practices and shift left the DevOps culture.

In his [[1974 interview with ABC
News]{.ul}](https://www.youtube.com/watch?v=sTdWQAKzESA), science
fiction author Arthur C. Clarke painted a picture of how computers would
change our way of life by the year 2001. One of his many extraordinarily
accurate predictions: 

"*Any businessman, any executive, could live almost anywhere on Earth
and still do his business through a device like this*." 

While there are some companies that have gone fully remote, (such as
[[gitlab]{.ul}](https://about.gitlab.com/company/culture/all-remote/)) a
large number of other organizations have tried to extend the
organizations in a remote site with clear goals. This approach is
primarily driven by 3 business drivers: **talent, cost and resiliency**.
Some of the companies such as start-ups as well as intrapreneurial
ventures start the remote site from Day 0, while others build a remote
team to scale the operations after the product has reached a certain
mature state. While there are some overlapping challenges with both
these cases, they also have unique nuances. I will try to cover them
both

**Desired State of an Organization**
--------------------------------------

Melvyn Conway, a computer scientist  who famously said that 

*"**Organizations** which design systems are constrained to produce
designs which are copies of the communication **structures** of these
**organizations**." *

Even though he mentioned this in 1967, Melvyn's law as it is called is
very much applicable even today. I also highly recommend you go through
this blog by Ashley-Christian Hardy on Agile team organizations
[[\[here\]{.ul}]{.ul}](https://medium.com/@achardypm/agile-team-organisation-squads-chapters-tribes-and-guilds-80932ace0fdc)
While each org may name them differently, the article pretty much
captures the essence of modern day agile teams. 

Here are some of the key concepts that you may want to consider for the
final **desired state** of your Org and work towards evolving them. 

**Scrum Team (sometimes also called a Squad)**
==============================================

This is the basic building block of an Org. They may be responsible for
building a service, module or a micro service. A scrum team (or squad)
owns the feature end to end: i.e. The team has the skills and tools to
design, develop and test the features. It is highly desirable that the
team members are polyglots and comfortable with UI and Backend
development as well picking up Test Automation. 

In addition to delivering the features, the team is also responsible for
managing their tech debt, participating in migrations and pager duty. 
The team also has a designated Scrum master(execution focussed), Product
Owner (Converting the Business Asks to backlog and prioritize), Tech
Leads (Who owns the architecture and design of the features) roles. 

A typical team is around 8-10 members that includes the manager. The
team needs to have a good mix of senior, mid-level and junior engineers.
This may look like around 3 junior engineers say with 0 \~ 3 Years of
experience, 3-4 mid-level engineers say with 3 - 8 years of experience
and tech leads and managers that typically have 10+ years of experience.
While the number of years of experience is only indicative, the managers
are the best judges to bring in the talent at the right level to the
team. 

**Building an uniform experience - (Virtual teams or Chapter)**
========================================================================

If every scrum team independently builds their feature in silo, the
customer experience will not be the same for the product. Some of the
examples of cross cutting experiences include UX/UI, API, Security,
Serviceability, Performance/Responsiveness, Upgrade etc. Typically the
platform scrum team is responsible for building component libraries that
are reusable by other scrum teams but this need not be the norm. To
ensure there is uniformity, an architect/lead works across all the scrum
teams to educate and ensure consistency across all the features. He can
also act as the gatekeeper to ensure the quality and consistency is met
across all the features. Functioning of the virtual teams is extremely
critical for a healthy product.

**Triad**
==========

A triad is a leadership team that creates balance between business,
customer and product. It consists of an architect, product manager and
engineering lead. You should not only see this partnership in the
highest level, but it should also bubble down to individual scrum teams.
Here are the typical roles of the triad

The Product Manager represents the customers, wider business and
stakeholders and vice versa, 

The Engineering Lead delivers the features to the product, ensures
reliability and stability and responds to issues. 

The architect makes the product coherent and provides the voice and
personality for the product. 

When triads work together, there is magic and there are no blame games.
It is important for leaders to have a good working relationship that is
built on trust, mutual respect and accountability.

**Agile vs Dev\|Sec\|ML\|Opsers vs SRE**
=========================================

There is enough debate on the overlaps or why one approach is better
than another. In my observation, when the org is small and the product
is in the early stages, there isn't a clear cut boundary on who does
what. All of them are considered software engineering problems with a
cultural aspect to winning the customer's trust. But as the product and
org grows, the team tries to build specialization around these areas and
fine tune the org. However it is important that the teams continue to be
customer centric, build on systems thinking and have a feedback loop
mechanism to continuously improve and respond in an agile fashion.

In a large organisation, a development team typically focuses on
building key capabilities that a customer would want. If I were to take
an analogy of Tesla, this may be a battery engineer who is working hard
to enhance battery life goals. 

The Dev/Sec/ML Opsers team is typically responsible for building
software that enables Continuous Integration and Continuous Delivery of
the Service or the Product. In most occasions, they build software
platforms and other scrum teams rely on for CI/CD. They also take a
broader view of the product and customer use cases to ensure the product
meets the needs at scale. Taking the same Tesla analogy, think of it as
the set of Manufacturing engineers that puts together the Car in the
Factory and delivers it to the Customers.

An SRE team is responsible for monitoring, availability, performance,
efficiency, emergency response and capacity planning of the Service. SRE
is typically applicable for the Cloud Services world. In the On-Prem
world, the customer experience team is responsible for responding to
customer requests and fixing the issues. Think of it as the modern
version of service engineering that leverages telemetry to fix your car
and keep it running without you even noticing it. 

**Stages of Team Development**
=========================================

It takes effort to build the right team. Here are the stages as referred by
Bruce Wayne Tuckman. This assume the product has a market acceptance and 
teams are not pivoting any more 

**Forming**
At this stage, individuals are trying to get to know each other and the org.
A leader can help by settitng up team building activities help gel in with the team. 
Assuming a new team, it will be good for the team to get hands on experience 
say by fixing bugs, documenting architecture etc.

**Storming**
This is a rocky stage. A lot of questions about the team identity will come up. 
The leader should lay out the vision for the team and communicate it often with 
the team. A team that works together stays together So start the team owning
the modules. Try to pair up a senior engineer with juniors

**Norming**
This stage is when the team starts to gel together. The team is ready to own up
a service or a particular feature. The leader should be able to convince people
that the team has reached the prime stage of expertise and can do magic

**Performing**
The team is well performing towards the goals. The leader can start delegating
to the team and groom next set of leaders

**Move the work and not the team**
It takes effort to build the team. So don't move the team members to work on 
random things. If the team is in a cruise control mode, move new work to the 
instead

In the next blog, I will discuss why the desired state cannot be
achieved on day zero at a remote site and how to go about fixing them

