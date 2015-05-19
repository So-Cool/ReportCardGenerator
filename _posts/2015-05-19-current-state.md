---
layout: post
title:  "Current State"
date:   2015-05-19 15:20:00
categories: maintenance status
---

I talked to my colleague managing the OpenEASE server instance. He's
looking into getting the `real` module on the server.

The report card should initially display information about the fetch
and place experiments. This is a defined environment for development
and testing of the report functionalities. Later on, this can be
extended to other experiments.

I'd like to talk about the information that will be included in the
reports on a pretty high level tomorrow; to define an overview of
expectations. This would include:

* Most important actions during the experiment: This can be based on a
  list of important task types

* General statistics: How long did the experiment take, how much time
  was spent on motion planning, how much on navigation/perception
  etc. This should be limited to those high level concepts to allow a
  quick glimpse on the "quality" of the currently used algorithms
  during the experiment.

* Special failure display: The robot couldn't grasp an object when
  tried (and out of what reason it failed). The logs include
  information about failures thrown by the robot. If an action failed,
  but no failure was attached, this is considered an anomaly and
  identifies a problem in the plans.

This is not an exhaustive list, but just a set of ideas. We can
discuss those on a high level to decide on a direction to take. Let me
know about your thoughts on that tomorrow (and also about more
concrete ideas you had so far).