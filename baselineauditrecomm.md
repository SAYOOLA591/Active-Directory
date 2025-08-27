# Baseline Microsoft Security Auditing with Group Policy Management

By default, Windows systems only log a limited set of events, resulting in significant visibility gaps. For example, many environments are not following the recommendations, and default auditing may miss critical actions, such as privilege escalation, Kerberos abuse, or lateral movement. I'll show you what you can enable to follow Microsoft's Audit Policy recommendations, and I'll also include the link below.

To address this, Microsoft provides recommended audit policy baselines that can allow administrators within the domain to just simply update a single policy and push it out to multiple computers within that domain saving a lot of time because you can imagine how long it would take to manually configure a workstation one by one especially if you have let's say 500 workstations so instead of doing that manually we can use GPO policy to push it across devices to do that we want to click on the Start menu and search for a group Policy Management.

[Policy Recommendation](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/security-best-practices/audit-policy-recommendations)
