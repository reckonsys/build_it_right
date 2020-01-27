# Python Log Level Considerations

## logging.basicConfig
In a Django application, consider whether or not the usage of `logging.basicConfig` will conflict with logging settings configured in the Django settings file. For example, if a module contains `logging.basicConfig(level=logging.INFO)` and a root logger is configured to log at _DEBUG_, what happens when the following is executed in the module: `log.debug()`? Is the log statement suppressed?

It's important to ensure that logging configuration can be modified via the settings file and not overridden by hard coded configuration.

### Question
Do we really need to use this method at all in Django apps? If so, why?

## Log levels
Ensure exceptions are logged using `log.exception`.

### Logging errors at INFO level
Consider the following:
```
    try
        ...some code
    except Exception as error:
        log.info(error)
```
Here's an example of how an error looks when logged at _INFO_ level:
```
INFO [2017-10-27 10:48:04,326] /abc/app/xyz/views.py [1533 140735838659520] list index out of range 
```

This example has two problems:

1. An error is logged at _INFO_ level. Suppose a monitoring tool is checking log files by filtering on the log level, e.g. _INFO_, _ERROR_, etc. Also suppose it alerts on _ERROR_ only. In this example the alert would never be triggered.
2. It only logs the error message. It's critical to capture full stack traces and other relevant context when errors occur, not just messages.


### Logging errors at ERROR level
The following example fixes these issues:
```
    try
        ...some code
    except Exception as error:
        log.exception(error)
```
Here's an example of the same error above, when logged at _ERROR_ level, using `log.exception`:
```
ERROR [2017-10-27 10:58:38,933] /abc/app/xyz/views.py [1702 140735838659520] list index out of range 
Traceback (most recent call last):
  File "/abc/app/xyz/views.py", line 356, in some_function
    today = context['upcoming'][0].start
  File "/.virtualenvs/abc/lib/python2.7/site-packages/django/db/models/query.py", line 201, in __getitem__
    return list(qs)[0]
IndexError: list index out of range
```

Also, the python logging library contains two methods for logging errors: `log.error()` and `log.exceptions()`. Both log at _ERROR_ level, however only one (exception method) will log a full stack trace.
