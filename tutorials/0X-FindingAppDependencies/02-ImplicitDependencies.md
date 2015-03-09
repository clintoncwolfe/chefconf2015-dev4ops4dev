# Finding IMPLICIT app dependencies

All the stuff no one thought of.

## Problem

So, now you know what the app needs to run, according to the box it
came in.  But is that really everything?

## Exercise

What else is needed to live with the app in production?

Assume you are deploying to (TODO: selected cloud provider).  What
else do you need - services, plans, and configuration - to stand up the systems?

## Discussion

TODO - solution.md

## Theory

### Meet The Ables

A well-behaved application has these abilities:

* Accessible
* Available
* Deployable
* Diagnosable
* Monitorable
* Promotable
* Recoverable
* Scalable
* Securable
* Testable

PRO TIP: To remember these, remember, if the app isn't
operationalized, you'll be RAD, MAD PSST!

#### Accessible

You, or your automated minions, need a way in. 

* If in a cloud, API/console access.  For AWS, who manages IAM users?
* SSH access.  Are you managing keys on a shared account?  You're not sharing keys, right?
* Are you using some kind of central identity management, like LDAP?
* What do the Network ACLs / Firewalls / Security Groups look like? Who manages them?  Do you need a private VPN tunnel onto the management subnet?
* What about network interconnects for the app itself (database to app server, etc)
* Does any of this have to be audited for PCI, SOX, PII, HIPAA?

#### Available

If something breaks, you need to have a plan for how the system
absorbs the shock.  

* What parts are likely to break or disappear?
* Which parts of the design are single points of failure?  Can you make them redundant?  At a justifiable cost?
* Determine the failure domains of your hosting provider.  These might be called Availability Zones, racks, circuits, or something else.  Which parts of your app should be contained in one FD?  Which parts should span FDs?  Search for hidden dependencies!
* How will you test your availability solutions?  Google "simian army" for ideas.

#### Deployable

Getting the thing setup for the first time, and rolling out new versions, can be really complicated.  Automation can be help here.

* What is your approach?  Are you shipping containers, or are you building each node using configuration mangement software like Chef?  If you are shipping containers, how are you building them?
* Do you always create new machines and deploy to them, or do you perform "upgrades" on existing machines to roll out new code?  
* How will you handle state - persistent business data, like the contents of databases?
* How will you handle schema changes in databases? (no one seems to be good at this yet)
* Does the app use "feature flags"?  How will you toggle them?
* How will you orchestrate things like service restarts?  Adding and removing members from a load balancer?

#### Diagnosable

Depending on the security policy, developers may or may not be permitted direct access to production instances.  If an application outage occurs, the developers will likely be the best people to fix the issue.

* How will logs from the instances be collected, retained, and made searchable?
* HOw will sensitive data be identified and removed from logstreams?
* Is it easy for developers to add instrumentation to the application?  Does it require a code push, or can it be done ad-hoc during an incident?
* Is relying on an external APM SaaS vendor like NewRelic or AppDynamics acceptable?
* Can you get the same instrumentation mechanisms in dev as in prod?  Can you turn off expensive metrics on a per-env basis?

#### Monitorable

TODO - we can reduce the number of alerts if we just stop monitoring

#### Promotable

TODO - moving versioned code artifacts in a controlled manner between
environments; can also address rollback, if you are a person who thinks
that is a thing that humans can actually do

#### Recoverable

TODO - I accidentally the data

#### Scalable

TODO - both more of the things and less of the things.  Predict cost,
align cost to demand, decide auto the autoscaling should be

#### Securable

TODO - "Seems Legit"

#### Testable

TODO - how do we prevent passing defective work down the line?  Infra
is code, infra must be testable.  Ephemeral machines, ephemeral test
envs.  Longevity testing; Decker did you see the incept date?

