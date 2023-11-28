# sbank Example Commands

Below is a set of helpful commands to help you better manage the projects you have running at the LCRC.

## View your project's allocations

### **Command:** sbank-list-allocations

Use this command to list all of your active allocations for a specific project [support]. This is useful when you need to provide this information in a report.

```sh
$ sbank-list-allocations -p support -r all
 Allocation  Suballocation  Start       End         Resource  Project  Jobs  Charged  Available Balance
 ----------  -------------  ----------  ----------  --------  -------  ----  -------  -----------------
 2           2              2023-10-01  2024-01-01  improv    support   961  1,456.5            8,621.6
 116         116            2023-10-01  2024-01-01  improv    support     3      7.0              496.9

Totals:
  Rows: 2
  Improv:
    Available Balance: 9,118.5 node hours
    Charged          : 1,463.5 node hours
    Jobs             : 964

All transactions displayed are in Node Hours. Each Node Hour is 128 Core Hours. Balances and transactions displayed will update every 5 minutes.
```

### List all active allocations for all resources for project ProjectX and add the field Created ﻿to the display list

```sh
$ sbank-list-allocations -r all  -p ProjectX -f "+created"
 Id         Start       End         Resource   Project          Jobs        Charged          Available Balance  Created    
 ---------  ----------  ----------  ---------  ---------------  ----------  ---------------  -----------------  ---------- 
 279        2011-08-30  2020-01-01  theta      ProjectX              6,361     12,332,699.9      -12,332,699.9  2013-02-22 
 2106       2016-01-04  2017-01-01  cooley     ProjectX              1,150          6,080.9           43,919.1  2016-01-04  

Totals:
  Rows: 2
  Theta:
    Available Balance: -12,332,699.9 node hours
    Charged          : 12,332,699.9 node hours
    Jobs             : 6,361 
  Cooley:
    Available Balance: 43,919.1 node hours
    Charged          : 6,080.9 node hours
    Jobs             : 1,150 
```

### List all available fields for the sbank-list-allocations command

```sh
$ sbank-list-allocations  -f "?"
available fields:
 allocation
 suballocation
 start_timestamp
 end_timestamp
 resource
 project_name
 jobs_count
 charged_sum
 available_balance_sum
 allocation_created_timestamp
 award_category
 award_type_name
 admin_name
 subname
 primary
 restricted
 deposits
 pullbacks
 sub_deposits
 sub_withdraws
 disable_message
 submanagement_enabled
 users_list
 comment
 last_updated_timestamp
```

## View your project's users

### **Command:** sbank-list-users

List all charges for userx on project support

```sh
$ sbank-list-users -p support -u userx
 User             Jobs        Charged         
 ---------------  ----------  --------------- 
 userx                 1,814          9,884.5

Totals:
  Rows: 1
  Resources: improv
  Projects: support
  Charged: 9,884.5 node hours
  Jobs   : 1,814 
```

### List charges for all users in support.

This works for project leads (i.e. PIs, Co-PIs, Proxies), since they can see everything in their own projects.

```sh
$ sbank-list-users -p support
 User             Jobs        Charged         
 ---------------  ----------  --------------- 
 user1                   120          4,243.7 
 user2                     0              0.0 
 user3                     0              0.0 
 user4                   181          1,195.5 
 user5                     0              0.0 
 user6                 2,560         10,868.7 
 user7                     0              0.0 
 user8                     0              0.0 
 user9                     0              0.0 
 user10                    7              3.5 
 user11                    0              0.0 
 

Totals:
  Rows: 11
  Resources: improv
  Projects: support
  Charged: 16,311.4 node hours
  Jobs   : 2,868 
  
```

## View your project's jobs

List jobs for user "userx" for jobs that started in the range 2016-02-15<= started < 2016-02-29 and add the transactions related to the job

### **Command:** sbank-list-jobs

**Note:** The job with the refund ```transaction_ids_list field can be shorten all the way to "t" in the -f "+ t"```

```sh
$ sbank-list-jobs -u userx -f "+ t" -S "2023-10-01...2023-11-20"
 Eventid  Jobid       Resource  Project          Allocation  Suballocation  User      Duration  Charged  Transaction Ids
 -------  ----------  --------  ---------------  ----------  -------------  --------  --------  -------  ---------------
 ...
 5488     6333.imgt1  improv    support          2           2              userx     0:01:26       2.4  CHARGE-5408
 5710     6557.imgt1  improv    support          2           2              userx     0:00:27       0.8  CHARGE-5555
 5711     6558.imgt1  improv    support          2           2              userx     0:00:26       0.7  CHARGE-5556

Totals:
  Rows: 317
  Improv:
    Charged      : 792.8 node hours
Date  filters (UTC):"2023-10-01 00:00:00" <= start < "2023-11-20 00:00:00"
```

### List the nodes used, runtime and start timestamp for Improv job 744160

**Note**: To display the date and time we increased the the number of characters of start_timestamp to 19

```sh
$ sbank l j -r improv -j 50576 -f "jobid nodes_used runtime start_timestamp:19" Jobid Nodes Used Runtime Start --------- ---------- --------- ------------------- 50576 512 1:00:49 2013-01-16 21:49:30 Totals: Rows: 1
```

## View your project's transactions

### **Command:** sbank-list-transactions

List of transactions that where at or after 2016-02-29 for ProjectX add fields: job_duration, nodes_used and hosts

**Note**:

- job_duration, nodes_used and hosts are shorten, but they are still uniquely identified
- host has the left justified width of 20, specified as "h:-20"

```sh
$ sbank-list-transactions -p ProjectX --at "ge 2016-02-29" -f "+ job_d nodes_u h:-20" -r theta
 Id         Resource   Project          Allocation  At          User             Transaction Type  Amount           Jobid      Job Duration  Nodes Used  Hosts                
 ---------  ---------  ---------------  ----------  ----------  ---------------  ----------------  ---------------  ---------  ------------  ----------  -------------------- 
 1025426    theta       ProjectX         2147        2016-02-29  userx            CHARGE                   48,005.1  740587     1:27:54       2048        MIR-00800-33BF1-2048 
 1028046    theta       ProjectX         2147        2016-03-01  userx            CHARGE                  147,647.1  742090     4:30:21       2048        MIR-40000-733F1-2048 
 1028755    theta       ProjectX         2147        2016-03-02  userx            CHARGE                1,576,068.0  742126     6:00:44       16384       MIR-04000-77FF1-1638 

Totals:
  Rows: 3
  Theta:
    Charges Amount: 1,771,720.2 node hours
    Job Duration  : 11:58:98 
Date  filters (UTC) : at >= "2016-02-29 00:00:00",  
```
