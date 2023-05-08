API INFRASTRUCTURE OUTAGE
In the course of this week I experienced an outage in my API infrastructure. Today I am providing an incident report that details the nature of the outage and my response.

The following is the incident report for the Google API infrastructure outage that occurred on January 30, 2023. I understand this service issue has impacted my valued developers and users, and I apologize to everyone who was affected.

Issue Summary

From 6:26 PM to 7:58 PM PT, requests to most Google APIs resulted in 500 error response messages. Google applications that rely on these APIs also returned errors or had reduced functionality. At its peak, the issue affected 100% of traffic to this API infrastructure. Users could continue to access certain APIs that run on separate infrastructures. The root cause of this outage was an invalid configuration change that exposed a bug in a widely used internal library.

Timeline (all times Pacific Time)
5:15 AM: Configuration push begins
5:30 AM: Outage begins
5:36 AM: Pagers alerted teams
5:50 AM: Failed configuration change rollback
6:05 AM: Successful configuration change rollback
6:20 AM: Server restarts begin
6:35 AM: 100% of traffic back online
Root Cause

At 5:15 AM PT, a configuration change was inadvertently released to the production environment without first being released to the testing environment. The change specified an invalid address for the authentication servers in production. This exposed a bug in the authentication libraries which caused them to block permanently while attempting to resolve the invalid address to physical services. In addition, the internal monitoring systems permanently blocked on this call to the authentication library. The combination of the bug and configuration error quickly caused all of the serving threads to be consumed. Traffic was permanently queued waiting for a serving thread to become available. The servers began repeatedly hanging and restarting as they attempted to recover and at 5:30 AM PT, the service outage began.



Resolution and recovery

At 5:36 AM PT, the monitoring systems alerted the engineers who investigated and quickly escalated the issue. By 5:40 AM, the incident response team identified that the monitoring system was exacerbating the problem caused by this bug.

At 5:55 AM, we attempted to rollback the problematic configuration change. This rollback failed due to complexity in the configuration system which caused our security checks to reject the rollback. These problems were addressed and we successfully rolled back at 6:25 AM.

Some jobs started to slowly recover, and we determined that the overall recovery would be faster by a restart of all of the API infrastructure servers globally. To help with the recovery, we turned off some of our monitoring systems which were triggering the bug. As a result, we decided to restart servers gradually (at 6:21 AM), to avoid possible cascading failures from a wide scale restart. By 6:50 AM, 25% of traffic was restored and 100% of traffic was routed to the API infrastructure at 6:59 AM.

Corrective and Preventative Measures

In the last two days, we’ve conducted an internal review and analysis of the outage. The following are actions we are taking to address the underlying causes of the issue and to help prevent recurrence and improve response times:
Disable the current configuration release mechanism until safer measures are implemented. (Completed.)
Change rollback process to be quicker and more robust.
Fix the underlying authentication libraries and monitoring to correctly timeout/interrupt on errors.
Programmatically enforce staged rollouts of all configuration changes.
Improve process for auditing all high-risk configuration options.
Add a faster rollback mechanism and improve the traffic ramp-up process, so any future problems of this type can be corrected quickly.
Develop better mechanism for quickly delivering status notifications during incidents.
Google is committed to continually and quickly improving our technology and operational processes to prevent outages. We appreciate your patience and again apologize for the impact to you, your users, and your organization. We thank you for your business and continued support.

