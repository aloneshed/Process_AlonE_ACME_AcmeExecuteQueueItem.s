### HmsTeam.ExecuteQueueItem.s Template ###
**ExecuteQueueItem.s**

* Built on top of *UiPath REFrameWork Template* template
* Uses *State Machine* layout for the phases of automation project
* Offers high level logging, exception handling and recovery
* Keeps external settings in Orchestrator assets
* Pulls credentials from Orchestrator assets
* Gets transaction data from Orchestrator queue and updates back status
* Takes screenshots in case of system exceptions


### How It Works ###

1. **BEGINNINGACTIONS PROCESS**
 + *BeginningActions* - Filling ProcessSetup variable with Assets, Job details, Environment properties etc.
 + *CheckForDownTime* - Checking if process may now run or not, depends on it's downtimes and it's business apps downtimes.
 + *StartProcess* - Performing actions that must run only once, at the very beginning of the process.

2. **INITIALIZE PROCESS**
 + *Initialization* - Open and login to applications used at the beginning of the process and after each trasaction system exception

3. **GET NEXT TRANSACTION ITEM**
 + *CheckForDownTime* - Checking if process may now run or not, depends on it's downtimes and it's business apps downtimes.
 + *GetNextTransactionItem* - Fetches transactions from an Orchestrator queue defined by argument("in_str_QueueName")

4. **EXECUTE TRANSACTION**
 + ./Framework/*ExecuteTransaction* - Process trasaction and invoke other workflows related to the process being automated 

5 **BEFORE SET TRANSACTION STATUS**
 + ./Framework/*BeforeSetTransactionStatus* - Performing actions before setting status. Exception here will cause transaction exception.
 
6. **SET TRANSACTION STATUS**
 + *SetTransactionStatus* - Updates the status of the processed transaction (Orchestrator transactions by default): Success, Business Rule Exception or System Exception

7. **AFTER SET TRANSACTION STATUS**
 + ./Framework/*AfterSetTransactionStatus* - Performing actions after setting status. Exception here will not cause transaction exception.

8. **BEFORE END PROCESS**
 + ./Framework/*BeforeEndProcess* - Performing actions that must run only once, at the end of the process, like sending email, report, etc.


### For New Project ###

1. Ensure existness of the Orchestrator required folders of Root\BusinessDepartment, Root\BusinessDepartment\General and Root\BusinessDepartment\BusinessProcess
2. In case you are using action center, ensure existness of a catalog in the relevant Orchestrator folder
3. Fill the queue name in argument in_str_QueueName
4. Implement ExecuteTransaction.xaml workflow and invoke other workflows related to the process being automated