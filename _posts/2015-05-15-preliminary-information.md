---
layout: post
title:  "Preliminary information & background"
date:   2015-05-15 13:36:25
categories: background CRAM
---

### Useful links ###
1. [openEASE][openEase]: this is where the result of your coding efforts will
be deployed.  
2. [Paper][openEasePaper]: an overview paper about openEASE.  
3. [Here][display]: `prolog/knowrob_vis.pl` holds the predicates currently available
for displaying information on openEASE.  
4. [Here][database]: the MongoDB interface for KnowRob (the underlying knowledge base "for robots"). Here, sub-symbolic (sensor) information from the log files is saved. `prolog/knowrob_mongo.pl` lets you look at the predicates currently available for getting sub-symbolic information from a log.  
5. [Here][logs]: access to the symbolic information extracted from logs; `prolog/knowrob_plan_logs.pl` holds the main predicates.  

* [Here][data]: example data file - see `cram_log.owl`.  
* [Here][dataAccess]: data access via web page.  
* [Semantic Hierarchical Recorder][smerc] (smerc): Open source tool for logs generation.  

### Running openEASE locally ###
[Here][PnP]: pick and place experiments. It includes:

* an OWL file `cram_log.owl`, reflecting the semantic structure of the experiment,
* a couple of `.json` files holding sub-symbolic information (TF, tf.json) and description entities (Designators, logged_designators.json); import them to MongoDB:
{% highlight BASH %}
mongoimport --db roslog --collection tf < tf.json
mongoimport --db roslog --collection logged_designators < logged_designators.json
{% endhighlight %}
* pictures taken by the robot during some actions, referenced in the OWL file.


Now install the OpenEASE and KnowRob locally via [Docker][docker]. This should already contain a sample pick and place experiment; for importing new ones, you will need to rebuild the Docker containers [here][dockerDev].

For forked GitHub repository the `dev` files are [here][gitdev] on the `indigo-devel` branch. `knowrob/docker` and `knowrob_web_tools`, both master branch, are also needed. All the rest is as described [here][devDescription].

---

[openEase]: open-ease.org
[openEasePaper]: {{ site.baseurl }}/docs/15-KISpecialIssueAL-openEASE.pdf
[logs]: https://github.com/knowrob/knowrob_addons/blob/indigo-devel/knowrob_cram/prolog/knowrob_plan_logs.pl
[display]: https://github.com/knowrob/knowrob/tree/indigo-devel/knowrob_vis
[database]: https://github.com/knowrob/knowrob/tree/indigo-devel/knowrob_mongo
[data]: http://www.robots-doing-things.com/incoming/log-packaged-2014-12-30-16-10-37.tar.gz
[smerc]: https://github.com/code-iai/semrec
[dataAccess]: http://data.open-ease.org/user/sign-in
[PnP]: http://www.robots-doing-things.com/incoming/log-packaged-2014-12-30-16-10-37.tar.gz
[docker]: http://knowrob.org/doc/docker
[dockerDev]: http://knowrob.org/doc/docker/dev
[gitdev]: github.com/knowrob/knowrob
[devDescription]: http://knowrob.org/doc/docker
