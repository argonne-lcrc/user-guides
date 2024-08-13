# sbank Example Commands

Below is a set of helpful commands to help you better manage the projects you have running on LCRC Improv.

All transactions displayed are in Node Hours. 1 Improv Node Hour is 128 Core Hours. 1 Bebop Node Hour is 36 Core Hours. Balances and transactions displayed will update every 5 minutes.

## View your Project’s Allocations
Command: `sbank-list-allocations [options]`

```
$ sbank-list-allocations -p <project_name>
Allocation  Suballocation  Start       End         Resource  Project  Jobs  Charged  Available Balance
----------  -------------  ----------  ----------  --------  -------  ----  -------  -----------------
2           2              2023-10-01  2024-01-01  improv    projectX    17     34.2             9965.8
116         116            2023-10-01  2024-01-01  improv    projectX     0      0.0              500.0

 Totals:
   Rows: 2
   Improv:
     Available Balance: 10465.8 node hours
     Charged          : 34.2 node hours
     Jobs             : 17
```

## View your Project’s Users
Command: `sbank-list-users [options]`

```
$ sbank-list-users -p <project_name>
 User       Jobs  Charged
 ---------  ----  -------
 user1       57    164.7
 user2       28     29.5
 user3      0      0.0

Totals:
  Rows: 3
  Resources: improv
  Projects: projectX
  Jobs   : 112
  Charged: 194.2 node hours
```

## View your Project’s Transactions
Command: `sbank-list-transactions [options]`

```
$ sbank-list-transactions -p <project_name>
...
 1661         improv    projectX  2           2              2023-10-26  user1     CHARGE       0.0  1752.imgt1
 1662         improv    projectX  2           2              2023-10-26  user1     CHARGE       0.0  1753.imgt1
 1663         improv    projectX  2           2              2023-10-26  user2     CHARGE       0.0  1754.imgt1
 1664         improv    projectX  2           2              2023-10-27  user2     CHARGE       0.0  1755.imgt1
 1665         improv    projectX  2           2              2023-10-27  user1     CHARGE       0.0  1756.imgt1

Totals:
  Rows: 135
  Improv:
    Charges Amount : 229.3 node hours
    Deposits Amount: 11500.0 node hours
```

## List Jobs Run by a User
Command: `sbank-list-jobs [options]`

```
$ sbank-list-jobs -u <username>
...
1611     1727.imgt1  improv    projectX          2           2              user1  0:00:26       0.1
 1612     1728.imgt1  improv    projectX          2           2              user1  0:00:28       0.1
 1613     1729.imgt1  improv    projectX          2           2              user1  0:00:26       0.1
 1614     1730.imgt1  improv    projectX          2           2              user1  0:00:26       0.1

Totals:
  Rows: 314
  Improv:
    Charged: 61.0 node hours
```

## Get Detailed Allocation Information
Command: `sbank-detail-allocations [options]`

```
$ sbank-detail-allocations -p <project_name>

-- Allocation Info:
* Allocation: 2
* Suballocation: 2
* Start: 2023-10-01 05:00:00
* End: 2024-01-01 05:59:59
* Resource: improv
* Project: projectX
* Jobs: 1,517
* Charged: 3,001.3
* Available Balance: 7,076.8
* Allocation Created: 2023-08-11 06:44:25
* Award Category: NA
* Award Type: NA
* Subname: None
* Primary: 1
* Restricted: 0
* Deposits: 10,078.1
* Pullbacks: 0.0
* Sub Deposits: 0.0
* Sub Withdraws: 0.0
* Disable Message: None
* Submanagement Enabled: 0
* Users: None
* Comment: None
* Last Updated: 2023-10-19 14:05:24
* Allocation History: No updates
* Suballocation History: No updates

-- Allocation Info:
* Allocation: 116
* Suballocation: 116
* Start: 2023-10-01 05:00:00
* End: 2024-01-01 05:59:59
* Resource: improv
* Project: projectX
* Jobs: 3
* Charged: 7.0
* Available Balance: 496.9
* Allocation Created: 2023-10-19 04:45:42
* Award Category: NA
* Award Type: NA
* Subname: None
* Primary: 1
* Restricted: 0
* Deposits: 503.9
* Pullbacks: 0.0
* Sub Deposits: 0.0
* Sub Withdraws: 0.0
* Disable Message: None
* Submanagement Enabled: 0
* Users: None
* Comment: None
* Last Updated: 2023-10-19 14:50:36
* Allocation History: No updates
* Suballocation History: No updates

Totals:
  Items: 2
  Improv:
    Available Balance: 7,573.8 node hours
    Charged          : 3,008.3 node hours
    Deposits         : 10,582.0 node hours
    Jobs             : 1,520
    Pullbacks        : 0.0 node hours
    Sub Deposits     : 0.0 node hours
    Sub Withdraws    : 0.0 node hours
```

If you are looking for a specific piece of information from sbank that is not listed above, please contact LCRC Support.
