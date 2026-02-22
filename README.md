# Homework 3

## Part 1 - Competition Rules

### Part 1.1: My Rule
**Rule 1 - *Interactions with grey team infrastructure that are not outlined as allowed will result in scoring deductions***

> Users red team may only interact with the scoring box's proxy and flag submission dashboard, while blue team may only interact with the wazuh dashboard web interface. Grey team infrastructure includes critical services and boxes that are used to properly administrate the competition. Interfering with any of the machines in a way that's out of scope for your team will result in point deductions at the discretion of team Foxtrot. 

*This will apply to both teams and their respective scopes.*

### Part 1.2: Purpose & Rationale

**Purpose**

> The purpose of this rule is to prevent either blue and red team from affecting grey team infrastructure in a way that's out of scope for their team. Blue team will have access to a wazuh dashboard that they can access on port 443 of wazuh-srv-01 with ip 10.10.10.7. Any other interaction with that machine from blue team is out of scope. If red team manages to get credentials for the dashboard, they may access the wazuh dashboard if they wish.