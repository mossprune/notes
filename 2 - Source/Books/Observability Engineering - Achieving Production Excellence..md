#computer-science #observability
[[2026-02-24]]

For a software to have observability, you must be able to do the following.

- Understand the inner workings of your application.
- Understand any system state your application may have gotten itself into, even new ones you have never seen before and couldn’t have predicted.
- Understand the inner workings and system state solely by observing and interrogating with external tools.
- Understand the internal state without shipping any new custom code to handle it (because that implies you needed prior knowledge to explain it)


A good litmus test for determining whether those conditions are true is to ask yourself the following questions:
• Can you continually answer open-ended questions about the inner workings of•
your applications to explain any anomalies, without hitting investigative dead
ends (i.e., the issue might be in a certain group of things, but you can’t break it
down any further to confirm)?
• Can you understand what any particular user of your software may be experiencing at any given time?
• Can you quickly see any cross-section of system performance you care about,•
from top-level aggregate views, down to the single and exact user requests that
may be contributing to any slowness (and anywhere in between)?
• Can you compare any arbitrary groups of user requests in ways that let you•
correctly identify which attributes are commonly shared by all users who are
experiencing unexpected behavior in your application?
• Once you do find suspicious attributes within one individual user request, can
you search across all user requests to identify similar behavioral patterns to
confirm or rule out your suspicions?
• Can you identify which system user is generating the most load (and therefore•
slowing application performance the most), as well as the 2nd, 3rd, or 100th most
load-generating users?
• Can you identify which of those most-load-generating users only recently started
impacting performance?
• If the 142nd slowest user complained about performance speed, can you isolate
their requests to understand why exactly things are slow for that specific user?
• If users complain about timeouts happening, but your graphs show that the•
99th, 99.9th, even 99.99th percentile requests are fast, can you find the hidden
timeouts?
• Can you answer questions like the preceding ones without first needing to
predict that you might need to ask them someday (and therefore set up specific
monitors in advance to aggregate the necessary data)?
• Can you answer questions like these about your applications even if you have
never seen or debugged this particular issue before?
• Can you get answers to questions like the preceding ones quickly, so that you
can iteratively ask a new question, and another, and another, until you get to the
correct source of issues, without losing your train of thought (which typically
means getting answers within seconds instead of minutes)?
• Can you answer questions like the preceding ones even if that particular issue has
never happened before?
• Do the results of your debugging investigations often surprise you by revealing
new, perplexing, and bizarre findings, or do you generally find only the issues
you suspected that you might find?
• Can you quickly (within minutes) isolate any fault in your system, no matter how
complex, deeply buried, or hidden within your stack?


Put simply, our definition of “observability” for software systems is a measure of how
well you can understand and explain any state your system can get into, no matter
how novel or bizarre. You must be able to comparatively debug that bizarre or novel
state across all dimensions of system state data, and combinations of dimensions, in
an ad hoc iterative investigation, without being required to define or predict those
debugging needs in advance. If you can understand any bizarre or novel state without
needing to ship new code, you have observability


We believe that adapting the traditional concept of observability for software systems
in this way is a unique approach with additional nuances worth exploring. For
modern software systems, observability is not about the data types or inputs, nor
is it about mathematical equations. It is about how people interact with and try to
understand their complex systems. Therefore, observability requires recognizing the
interaction between both people and technology to understand how those complex
systems work together.

Observability is the solution to that gap. Our complex production software systems
are a mess for a variety of both technical and social reasons. So it will take both social
and technical solutions to dig us out of this hole. Observability alone is not the entire
solution to all of our software engineering problems. But it does help you clearly see
what’s happening in all the obscure corners of your software, where you are otherwise
typically stumbling around in the dark and trying to understand things.


Observability requires evolving the way we think about gathering
the data needed to debug effectively. 

For the past two or three decades, the space between hardware and its human
operators has been regulated by a set of tools and conventions most call “monitoring.”
Practitioners have, by and large, inherited this set of tools and conventions and
accepted it as the best approach for understanding that squishy virtual space between
the physical and their code. And they have accepted this approach despite the knowledge that, in many cases, its inherent limitations have taken them hostage late into many sleepless nights of troubleshooting. Yet, they still grant it feelings of trust, and
maybe even affection, because that captor is the best they have.

With monitoring, software developers can’t fully see their systems. They squint at
the systems. They try, in vain, to size them up and to predict all the myriad ways
they could possibly fail. Then they watch—they monitor—for those known failure
modes. They set performance thresholds and arbitrarily pronounce them “good” or
“bad.” They deploy a small robot army to check and recheck those thresholds on their
behalf. They collect their findings into dashboards. They then organize themselves
around those robots into teams, rotations, and escalations. When those robots tell
them performance is bad, they alert themselves. Then, over time, they tend to those
arbitrary thresholds like gardeners: pruning, tweaking, and fussing over the noisy
signals they grow.


Many sophisticated apparatuses have been built atop the metric: time-series databases
(TSDBs), statistical analyses, graphing libraries, fancy dashboards, on-call rotations,
ops teams, escalation policies, and a plethora of ways to digest and respond to what
that small army of robots is telling you.
But an upper bound exists to the complexity of the systems you can understand
with metrics and monitoring tools. And once you cross that boundary, the change is
abrupt. What worked well enough last month simply does not work anymore. You
begin falling back to low-level commands like strace, tcpdump, and hundreds of
print statements to answer questions about how your system is behaving on a daily
basis.

It’s hard to calculate exactly when that tipping point will be reached. Eventually, the
sheer number of possible states the system could get itself into will outstrip your
team’s ability to pattern-match based on prior outages. Too many brand-new, novel
states are needing to be understood constantly. Your team can no longer guess which
dashboards should be created to display the innumerable failure modes.
Monitoring and metrics-based tools were built with certain assumptions about your
architecture and organization, assumptions that served in practice as a cap on com‐
plexity. These assumptions are usually invisible until you exceed them, at which point
they cease to be hidden and become the bane of your ability to understand what’s
happening. Some of these assumptions might be as follows:

• Your application is a monolith.
• There is one stateful data store (“the database”), which you run.
• Many low-level system metrics are available (e.g., resident memory, CPU load
average).
• The application runs on containers, virtual machines (VMs), or bare metal, which you control.
• System metrics and instrumentation metrics are the primary source of information for debugging code.
• You have a fairly static and long-running set of nodes, containers, or hosts to monitor.
• Engineers examine systems for problems only after problems occur.
• Dashboards and telemetry exist to serve the needs of operations engineers.
• Monitoring examines “black-box” applications in much the same way as local applications.
• The focus of monitoring is uptime and failure prevention.
• Examination of correlation occurs across a limited (or small) number of dimensions.

When compared to the reality of modern systems, it becomes clear that traditional
monitoring approaches fall short in several ways. The reality of modern systems is as
follows:
• Your application has many services.
• There is polyglot persistence (i.e., many databases and storage systems).
• Infrastructure is extremely dynamic, with capacity flicking in and out of existence elastically.
• Many far-flung and loosely coupled services are managed, many of which are not
directly under your control.
• Engineers actively check to see how changes to production code behave, in order
to catch tiny issues early, before they create user impact.
• Automatic instrumentation is insufficient for understanding what is happening
in complex systems.
• Software engineers own their own code in production and are incentivized to
proactively instrument their code and inspect the performance of new changes as
they’re deployed.
• The focus of reliability is on how to tolerate constant and continuous degradation, while building resiliency to user-impacting failures by utilizing constructs
like error budget, quality of service, and user experience.
• Examination of correlation occurs across a virtually unlimited number of•
dimensions.


The last point is important, because it describes the breakdown that occurs between
the limits of correlated knowledge that one human can be reasonably expected to
think about and the reality of modern system architectures. So many possible dimen‐
sions are involved in discovering the underlying correlations behind performance
issues that no human brain, and in fact no schema, can possibly contain them.
With observability, comparing high-dimensionality and high-cardinality data
becomes a critical component of being able to discover otherwise hidden issues
buried in complex system architectures.

Beyond that tipping point of system complexity, it’s no longer possible to fit a model
of the system into your mental cache. By the time you try to reason your way through
its various components, your mental model is already likely to be out-of-date.
As an engineer, you are probably used to debugging via intuition. To get to the source
of a problem, you likely feel your way along a hunch or use a fleeting reminder of
an outage long past to guide your investigation. However, the skills that served you
well in the past are no longer applicable in this world. The intuitive approach works
only as long as most of the problems you encounter are variations of the same few
predictable themes you’ve encountered in the past.


>Every application has an inherent amount of irreducible complexity. The only question is: who will have to deal with it—the user, the application developer, or the platform developer? 
>														—Larry Tesler

Modern distributed systems architectures notoriously fail in novel ways that no one is
able to predict and that no one has experienced before. This condition happens often
enough that an entire set of [assertions](https://en.wikipedia.org/wiki/Fallacies_of_distributed_computing) has been coined about the false assumptions
that programmers new to distributed computing often make. Modern distributed
systems are also made accessible to application developers as abstracted infrastructure
platforms. As users of those platforms, application developers are now left to deal
with an inherent amount of irreducible complexity that has landed squarely on their
plates