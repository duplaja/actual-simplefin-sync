Sync [ActualBudget](https://actualbudget.com/) via [SimpleFIN](https://beta-bridge.simplefin.org/)
## PREREQUISITES
  - A [SimpleFIN Token](https://beta-bridge.simplefin.org/) is required during **setup**
  - The [ActualBudget Budget ID](https://actualbudget.com/docs/developers/using-the-API/#getting-started) is required during **setup**
  - ActualBudget needs to be installed and running
  - Have your SimpleFin setup key handy. These are 1 time use only, so if for some reason you abort your setup early, you may need to generate a new one. If you re-run config, after a succesful setup, you won't need to change this unless you've deleted it from SimpleFin (stores the AccessKey returned).
  - Once you get everything running, you may (optionally) configure a system cron to run this automatically.

## IMPORTANT NOTES
 - SimpleFin data updates one time / day, for each linked account. The time that it updates may vary, even from day to day (based on the bank and upstream provider, MX).
 - The first run syncs back to the start of the current month. I would reconcile all accounts up to the start of the current month, either manually or via CSV import, and then run this.
 - You will want to check (for the first run) for any transactions right around the 1st of the current month, to make sure nothing is left out.
 - Future runs start from 5 days prior to the previous run, to catch any that processed later. No duplicate transactions will be created in Actual.
 - The max length of history returned varies from bank to bank, so I would make sure to run this monthly at a minimum. Some have a number of transactions limit, others return between 2-6 months of history.
 - You can reset all your settings by deleting the config.json file, and the budget folder, if you wish. This will not delete previous transactions from Actual.

## TODO
 - Better security for storage of SimpleFIN AccessKey
 - Allow custom date ranges for sync

## USAGE
  - **Sync** - If the app hasn't been configured yet, you'll be run through the setup steps, otherwise it will sync the current month for all accounts.
    ```
    node app.js
    ```

  - **Setup** - Change your SimpleFIN token, which budget file to use, your Actual Budget url, Actual Budget Password,and how the accounts are linked.
    ```
    node app.js --setup
    ```

  - **Link** - Change or add any new linked accounts
    ```
    node app.js --link
    ```

## TROUBLESHOOTING

- I've had some users report trouble using .local addresses to connect. If you run into this and are able, a reverse proxy might be of assistance.

- I've had a couple users report segfaults using Node v21.x. I haven't been able to recreate, but for those that have, switching to Node 20.x, deleting and re-cloning, and setting up again (with a new SimpleFin setup token) has fixed the issue both times.

- Be sure you have a budget, with encryption password (or leave blank with none), that has at least 1 account. You MUST have at least 1 account created, so you have something to map SimpleFin accounts to.
