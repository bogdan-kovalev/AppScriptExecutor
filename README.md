# AppScriptExecutor

Google Apps Script library that allows executing script functions at exact hour and minute.

# Install

You can also add it as a library.
https://developers.google.com/apps-script/guides/libraries  
Library's script ID: **12_fLujy5Nc-hxQWJD4nfFBriN1cU-NLjIuZVyW-NFsAzTdqXUAC8re11**

# Usage

```ts
// The functions that have to be executed at particular moments:

function RemindToDoAnExcercise() {
  console.log("my function that reminds to do an exercise triggered!");
}

function SaveAllWork() {
  console.log("my function that saves all work is triggered!");
}

function CreateMonthlyReport() {
  console.log("my function that creates a montly report is triggered!");
}

// Declaring the runtime context which holds the above functions.
const _runtimeCtx = this;

// @ts-ignore
AppScriptExecutor.SetContext({
  runtimeCtx: _runtimeCtx,
})

// @ts-ignore
const Executor = AppScriptExecutor.New({
  tasksGetter: {
    get() {
      // Tasks can be fetched from an external resource of your choice, for example Firebase database.
      // Using statically defined tasks as an example.
      const staticTasks = [
        "hourlyTask RemindToDoAnExcercise 2 8 18 everyday", // Executes `RemindToDoAnExcercise` function every 2 hours from 8 till 18
        "dailyTask SaveAllWork 19 0 weekDay", // Executes `SaveAllWork` function at 19:00 every week day
        "dailyTask CreateMonthlyReport 18 30 lastWeekDayOfMonth", // Executes `CreateMonthlyReport` function at 18:30 every last week day of month
      ];

      return staticTasks.map(AppScriptExecutor.TaskFromString);
    }
  }
})

```

## Advanced usage

```ts
function SendPayment() {
  console.log("my function that sends a payment triggered!");
}

// Tasks can be instantiated manually as well
// @ts-ignore
tasks.push(new AppScriptExecutor.DailyTask(
  "", // Task string, provided when parsed from string.
  SendPayment, // Function to execute
  13, // Hour
  25, // Minute
  () => new Date().getDate() % 2 == 0 // Allow to execute only on even calendar dates
))
```
