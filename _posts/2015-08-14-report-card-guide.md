---
layout: post
title:  "`report_card` guide"
date:   2015-08-14 16:50:21
categories: KnowRob
---

## The `report_card` package ##
[![Build Status](http://jenkins.ros.org/job/devel-indigo-report_card/badge/icon)](http://jenkins.ros.org/job/devel-indigo-report_card/): currently due to missing `knowrob` dependencies.  

The `report_card` is a CRAM package that extends [knowrob][knowrob] system and [openEASE][openEASE] environment with functionality of generating a PDF file that contains easy to read (e.g. pie charts, bar charts, tables) robot experiment statistics.

## Under the hood ##
This package is based on `Prolog` and its interpreter - the main log analysis and computations are done therein.  
Once the data is prepared part of it is passed to `R` via `real` interface where its processed and needed graphs are prepared. `R` is also used to store statistics and dump them into CSV and RData formats for easy post-processing.
Then, links to prepared figures and log statistics are passed via JPL to `Java` where they are prepared and injected into selected LaTeX templates with use of *Nixo Soft* [JLR library][JLR].  
Finally, the same library is used to generate a PDF based on prepared tex files. For details please refer to below figure.

<img src="https://raw.githubusercontent.com/So-Cool/ReportCardGenerator/gh-pages/docs/flowChart.png" alt="`report_card` flow chart" width="400" style="background-color:#509EB2;">

## Report card generation paradigm ##
To make the package the most universal the report card generation is based on two key concepts **report card layout templates** and **section templates**. This approach allows the user to choose the design of the report card and decide which sections should be placed within. Moreover, this design facilitates creating personalised section and including them in the report card without complex code alterations and rebuilding the whole package - see below.

### layout ###
This templates specify overall design of the report card - please see `report_card/report_card/tex_templates/reportCard.tex` for an example. The most important part of it are `\ifthenelse{ \equal{$section5}{} }{}{\input{$section5}}` statements which include specified in `Prolog` sections. The default layout allows up to 10 sections.

### sections ###
Section templates specify the structure of each section in the document - please see `report_card/report_card/tex_templates/overview.tex` for an example. There are 4 different types of variables that can be used in each section:

- $argument# - argument in which all LaTeX critical characters will be escaped - for use in normal text;
- $rawArgument# - argument which is not escaped - for use in *verbatim* environments and to pass file paths;
- $seqArguments# - sequences of arguments which are escaped;
- $rawSeqArguments# - sequences of arguments which are not escaped;

where # should be replaced with number 0 to 19 - currently up to 20 arguments of each type are allowed per section.  
For details on using arguments in the LaTeX sections please consult [JLR tutorial][JLR].

## Adding custom section ##
To add your custom section to the report card please create a file named `section_name.tex` in the `report_card/report_card/tex_templates` directory. Use the arguments naming as described above - the order of the arguments is based on the order used in the `Java` call done within `Prolog`.  

To generate your section from within `Prolog` add a new predicate to `report_card/prolog/report_card.pl` file:

{% highlight Prolog %}
generate_section_name(RcHomeOs, SectionPath) :-
  Section = 'section_name',

  code_to_get_all_statistics,

  jpl_datums_to_array([YourVariable1, YourVariable2], Strings),

  jpl_new('[Ljava.lang.String;', 0, RawStrings),

  jpl_datums_to_array([YourOtherVariable11, YourOtherVariable12], SeqStrings1),
  jpl_datums_to_array([YourOtherVariable21, YourOtherVariable22], SeqStrings2),
  jpl_datums_to_array([SeqStrings1, SeqStrings2], SeqStrings),

  jpl_new('[[Ljava.lang.String;', 0, RawSeqStrings),

  jpl_call( 'org.knowrob.report_card.Generator', section, [RcHomeOs, Section, Strings, RawStrings, SeqStrings, RawSeqStrings], SectionPath).
{% endhighlight %}

Where `RcHomeOs` is path to the temporary directory of current report card; `Section` is the section name (it MUST contain the exact name you gave to your section file without `.tex` extension), `Strings` and `RawStrings` are as described above and of type *array of strings* and finally, `SeqStrings` and `RawSeqStrings` are as described above and of type *array of array of strings*. If you have no data of specific type to be passed to the section creator you should generate empty array for the first two types e.g. `jpl_new('[Ljava.lang.String;', 0, Strings)` and empty array of arrays for latter two types e.g. `jpl_new('[[Ljava.lang.String;', 0, SeqStrings)`.  

---

Once your section generator is ready you either need to either alter the current report card generator or create your custom generator. You can also generate the report card manually: for details please see below.

#### Automated ####
To alter current generator please append your section to `generate_report_card(card_type(default))` predicate as follows:

{% highlight Prolog %}
generate_report_card(card_type(default)) :-
  % initialisation
  rc_temporary_directory(RcTempDir),
  create_directory(RcTempDir),
  prolog_to_os_filename(RcTempDir, RcHomeOs),

  % report card content
  generate_overview(RcHomeOs, Introduction),
  generate_actions(RcHomeOs, Actions),
  generate_statistics(RcHomeOs, Statistics),
  generate_failures(RcHomeOs, Failures),
  generate_summary(RcHomeOs, Summary),

  get_experiment_info(experimentId  , TrialId),

  %%%% custom content
  generate_section_name(RcHomeOs, CustomSection),
  jpl_datums_to_array([Introduction, Actions, CustomSection, Statistics, Failures, Summary], Strings),
  %%%%

  jpl_call( 'org.knowrob.report_card.Generator', rc, [RcHomeOs, TrialId, Strings], RcPdf),

  % data exporting and information outputting
  export_data(data_format(csv)),
  export_data(data_format(r)),
  write('Your report card is available at:\n'),
  write(RcPdf), !.
{% endhighlight %}

If you prefer to create custom report card generator first add type predicate to the already existing list: `card_type(custom_card_type).` and then create custom loader for created type:

{% highlight Prolog %}
generate_report_card(card_type(custom_card_type)) :-
  % initialisation
  rc_temporary_directory(RcTempDir),
  create_directory(RcTempDir),
  prolog_to_os_filename(RcTempDir, RcHomeOs),

  get_experiment_info(experimentId  , TrialId),

  % report card content
  generate_overview(RcHomeOs, Introduction),
  generate_actions(RcHomeOs, Actions),
  generate_statistics(RcHomeOs, Statistics),
  generate_failures(RcHomeOs, Failures),
  generate_summary(RcHomeOs, Summary),

  %%%% custom content
  generate_section_name1(RcHomeOs, Section1),
  generate_section_name2(RcHomeOs, Section2),
  generate_section_name3(RcHomeOs, Section3),
  jpl_datums_to_array([Section1, Section2, Section3], Strings),
  %%%%

  jpl_call( 'org.knowrob.report_card.Generator', rc, [RcHomeOs, TrialId, Strings], RcPdf),

  % data exporting and information outputting
  export_data(data_format(csv)),
  export_data(data_format(r)),
  write('Your report card is available at:\n'),
  write(RcPdf), !.
{% endhighlight %}

#### Manual ####
To manually generate the report card (using `Prolog` shell) please use the following sequence:

{% highlight Prolog %}
rc_temporary_directory(RcTempDir), create_directory(RcTempDir), prolog_to_os_filename(RcTempDir, RcHomeOs), get_experiment_info(experimentId  , TrialId), generate_section_name1(RcHomeOs, Section1), generate_section_name2(RcHomeOs, Section2), generate_section_name3(RcHomeOs, Section3), jpl_datums_to_array([Section1, Section2, Section3], Strings), jpl_call( 'org.knowrob.report_card.Generator', rc, [RcHomeOs, TrialId, Strings], RcPdf), export_data(data_format(csv)), export_data(data_format(r)).
{% endhighlight %}

Before generating a report card remember to load the data e.g. `load_experiment('/home/ros/datasets/ds4/cram_log.owl').`!

## Package requirements and installation ##
For details please refer to this [location][installation].

## Contributions ##
Any section contributions are welcomed, please make a pull request with you custom **report card generators** predicates, **report_card_type** declarations, **section.tex** and all additional predicates extending `report_card.pl`.  
Please document all your code and wrap it between *%%%%section_name - author - <%%%%* and *%%%%section_name - author - >%%%%*.

[JLR]: http://www.nixo-soft.de/tutorials/jlr/JLRTutorial.html
[knowrob]: http://knowrob.org
[openEASE]: http://www.open-ease.org
[installation]: http://so-cool.github.io/ReportCardGenerator/2015/05/29/development/

