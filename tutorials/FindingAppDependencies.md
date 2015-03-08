Find Application Dependencies

- Why do we care about this?

Most services are far more than simply a running process listening on a port somewhere.  

When an applictation is getting ready to be deployed, you need to know what you need to bring with you.

Some examples of things that the software DIRECTLY relies on might include:

Developers will have a great deal of information about these dependencies, but may have become so accustomed to them that they don't think to mention them.  Does the fish mention the water?

     - persistence
       - SQL DBs - postgres, mysql, etc
       - nosql
     - HTTP termination & load balancing
       - nginx / apache / etc
     - messaging
       - rabbitmq, JMS
     - if a public webservice, caching
       - memcached
     - Software-as-a-Service externalities
       - email transport services
       - one of many niche service that are app-domain-specific

How can the people responsible for deploying the application discover these dependencies, make sure the list is complete, and prepare for the deployment?

== Approaches that are Less Useful ==

- Go back in time

Of course, the decision to pull in a SQL server, make it Postgres, and make it 9.3, had to have ben made at aoms point.  In DevOps-heavy organizations, ops engineers are present in the design meetings.  They can help advice choices, start to plan for scaling and redundancy, and get in front of any anti-patterns that are obvious to Ops but less so to Dev.

So, ideally, you'd go back in time, and be at those meetings.  

Failing that, the earlier the deployers and operators can be involved in the development process, the better.  

- Read the up-to-date, easily found, very complete design document

Some organizations beleive that you can have detailed, thorough documentation that is also perfectly up to date.  That sounds great, but often - as schedules slip and feature pressue builds - documentation becomes deprioritized.  

By all means use any docs that you find, but bring along an interpreter, such a developer.

- Spend a day with a developer setting up an environment

Using screen sharing or physical desk sharing, "peer program" the experience of going from (your equivalent of) bare metal to a running, working application.  This will reveal many deltas from the "official plan," and may 

This can be a very productive approach, but requires some things that may be non-starters:
  - Setting up an environment may be an undocumented, multi-day affair
  - Developers may not set them up consistently - one uses Vagrant/VirtualBox, one uses direct execution on the local machine, one uses AWS.
  - The actual environment may have toy implementations:
    - sqlite instead of postgres
    - WEBrick instead of a real webserver
    - etc
  - getting a developer's time away from feature work may not be polically possible

= Some More Practical Approaches =

- Conversation with developers

  - Always push back to the mind of the beginner - devs will assume all sorts of things, becuse it was setup months ago and they don't have to think about it anymore.  Play ignorant, and keep saying things like "So a machine running ONLY postgres and nginx is all you need?  Just that one machine?"

- If configuration management code exists, read the deps 

If the project has Chef / Puppet / Ansible / etc code, dive in and read.

Two rich areas of exploration:
  
  - Instance-specific information, like Chef node runlists or Ansible playbooks.
  - Dependency data, like Berksfiles

- Examine the running instances

If real instances exist, login and look around.  

 - What services are running according to init.d?
 - What ports are listening, according to netstat -nlp --inet? 

- read the config files

Look for service locators / hostnames.  

Hey, they mention the RMQ hostname.  
And having a RO database host and a RW database host.  How are those synced?

- read the gemfiles / setup.py / npm.json

Whatever language(s) are used, they all have dependency managers.  If the app depends on RabbitMQ, it must pull in a RMQ client library at some point.  

This can lead to some sleuthing, as many language-specific de[endencies have cutesy names.  Some are more obvious than others - "bunny" being a RabbitMQ client - but others are rather opaque.  A modern app may have hundred of gem/module/NPM deps, and you have no context as to what is important or why.  It's also a major place for cruft - things that are no longer needed are rarely removed.  Having a developer at your side can make this process much easier.

---- marker

This doesn't include INDIRECT dependencies:


     - system services
       - LDAP
       - NTP

