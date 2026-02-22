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

> Consequences are determined entirely at Foxtrot's discretion. The consequences vary based on the severity of the actions taken. If (for example) red team manages to get into the deployment box and read through each of the ansible scripts to get some of the flags, that would potentially result in the flags not counting. Rule breaking will be discussed within grey team, and penalties and/or warnings will be given before the next competition day.

**Prevention**

> Some of the ways that machines will prevent other teams from accessing them is through allowing ssh only for GREYTEAM, changing the passwords, and not allowing either team to access the console of any of the machines (which is already done automatically). This isn't a perfect fix, but it fixes most of the risk associated with breaking this rule.

### Part 1.4: Real-World Justification

**Why is this rule necessary?**

> The purpose of this rule is to make sure the competition is more realistic for the participants. In a real-world scenario, red team would not be able to access the scripts inserting flags in plaintext and with all the details of how things are setup to be purposefully vulnerable. For blue team, they wouldn't be able to see where flags are placed and documentation on how services are made to be vulnerable. This is important for enforcing the usefulness of running the competition. The purpose is to gain experience through the competition, which this rule enforces.

## Part 2 - Assessment Criterion Analysis

Note that this isn't being actively used in competition 1 and is just being used for the purpose of this assignment. It's an issue that we're currently dealing with as blue team has broken SSH on at least one of the machines.

### Part 2.1: Criterion Statement

**Assessment**: Assessing the Status of SSH

**Description**

> Measures the availability of SSH on blue team boxes for the GREYTEAM user. The scoring engine will attempt to run a simple command via SSH to each of the blue boxes occasionally throughout the competition. This will apply to blue team and relates to business operations.

### Part 2.2: Importance & Justification

**Importance**

> The reason this is important is because SSH will be used by Foxtrot team to be able to do work inside each of the machines during/between the competitions. Shutting off or breaking SSH will affect how well Foxtrot will be able to administrate the competition. It teaches blue team to be careful when kicking out attackers. In a real-world scenario, even if attackers are in a machine, it may be necessary to keep critical services active while still preventing the attackers from doing more. Blue team will need to make sure they're not causing critical services to go down. This will protect the integrity of the competition. This also incentivises blue team being intentional with their actions rather than blindly relying on AI generated scripts doing things they don't understand. Blue team should understand what's being done and learn rather than relying on things that could potentially have drastic consequences in real-life situations.

**What would be lost if this wasn't assessed?**

> It would mean Foxtrot team wouldn't be able to access the boxes they need to administrate in some cases. Blue team shouldn't just immediately disable SSH to prevent red team from accessing it, they should be intentional in preventing only the wrong people from accessing the machines. Not having this rule also makes it easier for blue team to just disable ssh access or to follow AI scripts blindly without understanding them. Business continuity is important, and breaking services that will need to work once the incident is resolved only causes further problems when things can be fixed in other ways in many cases.

### Part 2.3: Measurement Methodology

**Sampling Frequency**

> This check will be done on every box, so the checks should be staggered so it's not SSHing to every single box at the same time. The script should go through the list of hosts, using SSH on a host every 10 seconds to verify that it's active. This frequency would mean that every box is checked at least once every 2 minutes (technically less).

**Measurement Process**

> This is done through a custom script that runs a whoami command on the boxes and determines whether the command was successful or not. In this case, it doesn't just determine whether SSH is active, it also ensures that the GREYTEAM user's password hasn't been changed and that Foxtrot team is able to access the machine at any time. This runs on the scoring engine box within the grey team infrastructure. It's done in the same network as both teams, but interactions that would break the affectiveness of this scoring method are prevented by the rule created in part 1 of this assignment. A success is the command returning GREYTEAM, and a failure would be the SSH command failing.

**Data Collection**

> The data from this will be stored using methods from the scoring engine. This can also be done via a database as well. The data stored would be success/failure and the output of the commands. It will also be checked immediately before and after each competition day to ensure integrity.

**Technical Implementation**

``` python
import subprocess
import time

ip_list = # list of ips that SSH needs to be active on

while True: # this will be set to while the scoring engine is active
    for ip in ip_list:
        result = subprocess.run([f"ssh", f"-o", f"ConnectTimeout=5", f"GREYTEAM@{ip}", f"\"whoami\""], capture_output=True, text=True)
        if result:
            # insert result.stdout and success to the database with the ip as the primary key
        else:
            # insert result.stdout and failure to the fatabase with the ip as the primary key
        time.sleep(10) # try the next ip in 10 seconds
```
**Log Appearance**

```
          ip          |          result          |                         output
-----------------------------------------------------------------------------------------------------------------
      10.10.10.21     |           true           |                        GREYTEAM
      10.10.10.22     |          false           | ssh: connect to host 10.10.10.22 port 22: Operation timed out
      10.10.10.23     |           true           |                        GREYTEAM
      10.10.10.24     |           true           |                        GREYTEAM
      10.10.10.25     |           true           |                        GREYTEAM
      10.10.10.26     |           true           |                        GREYTEAM
```
and so on...

### Part 2.4: Metrics and Scoring

**Metric Definition**

*Points are not gained by having SSH up, rather points are lost when SSH is down. This is based on uptime being the method for points being increased for blue team.*

``` python
Uptime_Percent %= ((Successful_Checks_All_Services / Total_Checks_All_Services) * 100) - ((SSH_Failure_Checks / SSH_Total_Checks) * 15)
Competition_Day = Uptime_Percent * 10
```

Note that SSH isn't a service that was made vulnerable. Blue team is simply tasked with not breaking it, and red team is not allowed to break it. Because it's not one of the services blue team is meant to protect, and red team will not be allowed to break it, it will be treated as a deduction rather than something that is scored when it's broken. If red team breaks it, we would need to manualy go in, calculate the points that blue team lost from SSH being down on some boxes, remove the deduction, and add the points that were originally lost on top of that. That means that red team breaking SSH will add to blue team's score. This isn't shown in the calculations because it will need to be done manually rather than automatically unless we setup a script to do so. With the way it's calculated, blue team can lose up to 150 points (or 15% of their score) if SSH is down on all machines throughout the entire competition.

**Scoring Examples**

```
     Scenario     |   Checks Failed   |    SSH Uptime    |  Day Points Earned    
-------------------------------------------------------------------------------
Perfect Uptime    |       0/630       |       100%       |        1000
SSH Broken on 1 ip|      70/630       |      88.89%      |       983.33
SSH Broken on 4 ip|      280/630      |      61.90%      |       933.33
SSH Broken on 6 ip|      420/630      |      33.33%      |        900
SSH Broken on 9 ip|      630/630      |        0%        |        850
```

This means that the maximum number of points that can be lost (total deduction amount) is 150 points. If red team breaks SSH on enough machines in the middle of the competition that the competition needs to be paused, the amount of time that the competition is paused for would also contribute to the number of points that blue team would gain. Blue team would also not be able to have more than 1000 points total per day of the competition.

**Fairness**

> This metric allows for SSH to not be seen as something that red should attack, and encourages blue team to be much more thoughtful about what commands they're entering. Because red team cannot break SSH, blue team doesn't need to worry about it being down unless they cause it to be. It encourages blue team to understand ssh configuration and authentication, and to understand what they're doing rather than blindly trusting scripts and commands they get from AI or the internet. This setup also prevents red team from gaming the system by breaking SSH because it damages their chances of winning the competition by adding to blue team's points when red team breaks SSH. Blue team cannot gain anything from breaking this as Foxtrot already has logs of what's happening on each machine. This means they can't break SSH and suddenly start breaking rules without us knowing and punishing them for it. Ambiguous cases are handled by viewing the logs and determining intent when all else fails. If the logs clearly show red team targetting SSH, then blue team gains the points. If the logs show (and this has happened in the competition already) that blue team breaks pam by messing with the configuration when nothing has been done to it, therefore breaking ssh authentication, blue team will lose points for breaking SSH by doing things they seemingly are running without understanding. The way that Foxtrot has setup this competition has allowed us to rely on our discretion if something fails, so if the metric fails, we'll speak with one another and determine how many points should be deducted or added. Based on how it is setup, 150 points is the maximum that can be added/deducted.

## References

There have been no references made. I used my experience from the setup and preparation of competition 1 as well as the examples given directly in the homework instructions (not the links) to complete this assignment.