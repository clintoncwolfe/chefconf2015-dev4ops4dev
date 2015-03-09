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

TODO - redundancy plan, SPOF analysis, multi-AZ, multi-region, etc

#### Deployable

TODO - the machines should unfold themselves; are you always deploying
to fresh instances or do you think incremental deploys are a good
idea; what about stateful instances / volumes

#### Diagnosable

TODO - ELK, APM, and friends; when something goes wrong in prod, will
the SMEs have visibility into the issue; sensitve data in logstreams;
retention

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

