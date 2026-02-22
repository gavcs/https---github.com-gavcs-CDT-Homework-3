# Homework 3

## Part 0 - Header Information

* Gavin McConnell
* Team Foxtrot
* Assignment: HW#3 - Competition Rules & Assessment Design


## Part 1 - Competition Rules

### Part 1.1: My Rule
**Rule 1 - *Interactions with grey team infrastructure that are not outlined as allowed will result in scoring deductions***

> Interfering with any of the machines in a way that's out of scope for your team will result in point deductions at the discretion of team Foxtrot. Blue team will have access to a wazuh dashboard that they can access on port 443 of wazuh-srv-01 with ip 10.10.10.7. Any other interaction with that machine from blue team is out of scope. If red team manages to get credentials for the dashboard, they may access the wazuh dashboard if they wish. Red team may use the proxy on the scoring box (10.10.10.6) to interact with blue team machines. Red team may also interact with the flag submission dashboard on the scoring box via HTTPS on port 443 (https://10.10.10.6/login).

*This will apply to both teams and their respective scopes.*

### Part 1.2: Purpose & Rationale

**Purpose**

> The purpose of this rule is to prevent either blue and red team from affecting grey team infrastructure in a way that's out of scope for their team. Grey team infrastructure includes critical services and boxes that are used to properly administrate the competition. This infrastructure includes scripts that run with the flag in plaintext directly within the code, SIEM dashboards that log what each team is doing during and outside of competition hours, the scoring engine, and the machine that contains the openstack and ansible scripts with all of the flags. This solves the problem of red team being able to access all of the flags in plaintext, the possibility breaking services that must stay on throughout the duration of the competition, and the ability of either team being able to view what Foxtrot team has done during the preperation of the competition and what they're doing during it.

**Rationale**

> The rationale behind this rule is to ensure that teams learn through the competition rather than reading through the scripts. It also ensures fairness between the teams by not allowing one team to get an advantage over the other by accessing competition administrative tools. This also protects the infrastructure and administration of the competition through not allowing either team to access management tools/infrastructure. In a competition with much more time to setup the infrastructure and to plan the layout, this infrastructure would be on a network that blocks interaction from blue or red team, but allows for the opposite. In this case, Foxtrot team needed to focus more on making the competition functional rather than perfect given the time constraints. This rule enforces it through point loss rather than not being possible.

### Part 1.3: Enforcement Mechanism

**Detection & Verification**

> With the current infrastructure, there's a wazuh SIEM with agents on all of the blue team machines, and a wazuh SIEM with agents on all of the red team machines. By making each of the machines on grey team infrastructure wazuh agents on the red team wazuh (which neither red nor blue team has access to) we're able to view logs on all of the machines. This means that if someone on either team does things that are not allowed, we'll see it and be able to read through logs to understand exactly what happened. This allows us to detect when rules are broken, and to verify exactly how they were broken.

**Consequences**

> Consequences are determined entirely at Foxtrot's discretion. The consequences vary based on the severity of the actions taken. If (for example) red team manages to get into the deployment box and read through each of the ansible scripts to get some of the flags, that would potentially result in the flags not counting. 