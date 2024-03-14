# lcrc-sbank Allocation Accounting System (Slurm Clusters)

**The `lcrc-sbank` commands are specifically designed for clusters that use Slurm. Make sure you're using these commands on the following LCRC clusters:**

- **Bebop**
- **Swing**

## Project Allocation Queries and Management

The commands listed below are essential for LCRC users to manage their project allocations on Slurm clusters. These tools allow you to query project balances, transactions, and set or change your default project. Remember, commands are cluster-specific and will only provide information for the cluster currently being accessed.

| Command                        | Description                                     |
| ------------------------------ | ----------------------------------------------- |
| `lcrc-sbank -q balance`        | Query all of your LCRC project balances.        |
| `lcrc-sbank -q balance <proj_name>` | Query a specific LCRC project balance.     |
| `lcrc-sbank -q default`        | Query your default LCRC project.                |
| `lcrc-sbank -s default <proj_name>` | Change your default LCRC project.         |
| `lcrc-sbank -q trans <proj_name>`   | Query all transactions on an LCRC project. |
| `lcrc-sbank -h`                | Print the help menu for the lcrc-sbank command. |
