TITLE: Scheduling a task using CommonJS
DESCRIPTION: This code snippet demonstrates how to schedule a task to run every minute using the node-cron module in a CommonJS environment. It imports the `node-cron` module, then uses the `cron.schedule` method to define the cron expression and the function to be executed.
SOURCE: https://github.com/node-cron/node-cron/blob/master/README.md#_snippet_1

LANGUAGE: JavaScript
CODE:
```
const cron = require('node-cron');

cron.schedule('* * * * *', () => {
  console.log('running a task every minute');
});
```

----------------------------------------

TITLE: Scheduling a task using ES6 (module)
DESCRIPTION: This code snippet demonstrates how to schedule a task to run every minute using the node-cron module in an ES6 module environment. It imports the `node-cron` module using the `import` syntax and then uses the `cron.schedule` method to define the cron expression and the function to be executed.
SOURCE: https://github.com/node-cron/node-cron/blob/master/README.md#_snippet_2

LANGUAGE: JavaScript
CODE:
```
import cron from 'node-cron';

cron.schedule('* * * * *', () => {
  console.log('running a task every minute');
});
```

----------------------------------------

TITLE: Stopping a scheduled task
DESCRIPTION: This code snippet demonstrates how to stop a currently running scheduled task using the `task.stop()` method. After stopping, the task will no longer be executed until it is restarted.
SOURCE: https://github.com/node-cron/node-cron/blob/master/README.md#_snippet_10

LANGUAGE: JavaScript
CODE:
```
import cron from 'node-cron';

const task = cron.schedule('* * * * *', () =>  {
  console.log('will execute every minute until stopped');
});

task.stop();
```

----------------------------------------

TITLE: Starting a scheduled task
DESCRIPTION: This code snippet shows how to start a scheduled task that was initially created with the `scheduled` option set to `false`. The `task.start()` method is called to begin the execution of the cron job.
SOURCE: https://github.com/node-cron/node-cron/blob/master/README.md#_snippet_9

LANGUAGE: JavaScript
CODE:
```
import cron from 'node-cron';

const task = cron.schedule('* * * * *', () =>  {
  console.log('stopped task');
}, {
  scheduled: false
});

task.start();
```

----------------------------------------

TITLE: Using step values in cron syntax
DESCRIPTION: This code demonstrates scheduling a task using step values in the cron expression. The cron expression '*/2 * * * *' specifies that the task should run every two minutes.
SOURCE: https://github.com/node-cron/node-cron/blob/master/README.md#_snippet_5

LANGUAGE: JavaScript
CODE:
```
import cron from 'node-cron';

cron.schedule('*/2 * * * *', () => {
  console.log('running a task every two minutes');
});
```

----------------------------------------

TITLE: Using multiples values in cron syntax
DESCRIPTION: This code demonstrates scheduling a task using multiple values in the cron expression. The cron expression '1,2,4,5 * * * *' specifies that the task should run every minute at minutes 1, 2, 4, and 5.
SOURCE: https://github.com/node-cron/node-cron/blob/master/README.md#_snippet_3

LANGUAGE: JavaScript
CODE:
```
import cron from 'node-cron';

cron.schedule('1,2,4,5 * * * *', () => {
  console.log('running every minute 1, 2, 4 and 5');
});
```

----------------------------------------

TITLE: Using ranges in cron syntax
DESCRIPTION: This code demonstrates scheduling a task using a range of values in the cron expression. The cron expression '1-5 * * * *' specifies that the task should run every minute from minute 1 to minute 5.
SOURCE: https://github.com/node-cron/node-cron/blob/master/README.md#_snippet_4

LANGUAGE: JavaScript
CODE:
```
import cron from 'node-cron';

cron.schedule('1-5 * * * *', () => {
  console.log('running every minute to 1 from 5');
});
```

----------------------------------------

TITLE: Scheduling a task with timezone and options
DESCRIPTION: This code demonstrates scheduling a task with specific options, including timezone and scheduling status. The task will run at 01:00 in the America/Sao_Paulo timezone, only if scheduled is set to true.
SOURCE: https://github.com/node-cron/node-cron/blob/master/README.md#_snippet_8

LANGUAGE: JavaScript
CODE:
```
 import cron from 'node-cron';

  cron.schedule('0 1 * * *', () => {
    console.log('Running a job at 01:00 at America/Sao_Paulo timezone');
  }, {
    scheduled: true,
    timezone: "America/Sao_Paulo"
  });
 
```

----------------------------------------

TITLE: Using short names for month and weekday in cron syntax
DESCRIPTION: This code demonstrates scheduling a task using short names for month and weekday in the cron expression. The cron expression '* * * Jan,Sep Sun' specifies that the task should run every Sunday in January and September.
SOURCE: https://github.com/node-cron/node-cron/blob/master/README.md#_snippet_7

LANGUAGE: JavaScript
CODE:
```
import cron from 'node-cron';

cron.schedule('* * * Jan,Sep Sun', () => {
  console.log('running on Sundays of January and September');
});
```

----------------------------------------

TITLE: Using names for month and weekday in cron syntax
DESCRIPTION: This code demonstrates scheduling a task using names for month and weekday in the cron expression. The cron expression '* * * January,September Sunday' specifies that the task should run every Sunday in January and September.
SOURCE: https://github.com/node-cron/node-cron/blob/master/README.md#_snippet_6

LANGUAGE: JavaScript
CODE:
```
import cron from 'node-cron';

cron.schedule('* * * January,September Sunday', () => {
  console.log('running on Sundays of January and September');
});
```

----------------------------------------

TITLE: Validating a cron expression
DESCRIPTION: This code snippet demonstrates how to validate a cron expression using the `cron.validate()` method. It checks if the given string is a valid cron expression and returns a boolean value.
SOURCE: https://github.com/node-cron/node-cron/blob/master/README.md#_snippet_11

LANGUAGE: JavaScript
CODE:
```
import cron from 'node-cron';

const valid = cron.validate('59 * * * *');
const invalid = cron.validate('60 * * * *');
```

----------------------------------------

TITLE: Naming a scheduled task
DESCRIPTION: This code snippet demonstrates how to assign a name to a scheduled task using the `name` option. This name can be used for identification and logging purposes.
SOURCE: https://github.com/node-cron/node-cron/blob/master/README.md#_snippet_12

LANGUAGE: JavaScript
CODE:
```
import cron from 'node-cron';

const task = cron.schedule('* * * * *', () =>  {
  console.log('will execute every minute until stopped');
}, {
  name: 'my-task'
});
```

----------------------------------------

TITLE: Listing all tasks
DESCRIPTION: This code snippet demonstrates how to retrieve a list of all scheduled tasks using the `cron.getTasks()` method. It then iterates through the tasks and logs the key and value for each task.
SOURCE: https://github.com/node-cron/node-cron/blob/master/README.md#_snippet_13

LANGUAGE: JavaScript
CODE:
```
import cron from 'node-cron';

const tasks = cron.getTasks();

for (let [key, value] of tasks.entries()) {
  console.log("key", key)
  console.log("value", value)
}
```

----------------------------------------

TITLE: Installing Node-cron with npm
DESCRIPTION: This command installs the node-cron package using npm, adding it as a dependency to your project. The `--save` flag ensures that the package is listed in your project's `package.json` file.
SOURCE: https://github.com/node-cron/node-cron/blob/master/README.md#_snippet_0

LANGUAGE: console
CODE:
```
npm install --save node-cron
```