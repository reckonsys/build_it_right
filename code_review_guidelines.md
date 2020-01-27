# Code Review Guidelines

This page captures common comments made when reviewing pull requests. This is a list of common mistakes, not a style guide.

# Style

In general you should follow existing style guides:

1. [PEP 8 -- Style Guide for Python Code](https://www.python.org/dev/peps/pep-0008/)
2. [Google Python Style Guide](https://google.github.io/styleguide/pyguide.html)

Most IDE tools have built in support for python code analysis. Make sure you are reviewing any issues highlighted in your IDE.

# Common Comments
Each section heading below can also be used as short hand when adding comments to a PR.

## Logging
1. Don't use print statements. Use python's built-in logging facility. See other code in the repo as an example.
2. Log errors at the correct level, either *log.error* or *log.exception*, depending on what you are logging. See [here](https://bitbucket.org/vndly/vndly/wiki/Python%20Log%20Level%20Considerations) for a detailed explanation.

## Exception Handling
1. Avoid use of generic exception handlers. See [also](https://google.github.io/styleguide/pyguide.html#Exceptions).
1. Not all exceptions need to be logged. If you're not sure ask. One example is Django's DoesNotExist exception. Generally this can be handled as an HTTP 404 without logging.

## Input Validation
1. All input sent to the server should be validated, regardless of whether it is validated in the browser.
2. Input validation errors are generally considered HTTP 400 errors. When an error occurs, do not log it and return an appropriate error message to the user of the application.
3. Django Serializer errors are generally considered input validation errors.

## Transactions
A server request that triggers multiple writes to a relational database should use a transaction. See Django's [documentation](https://docs.djangoproject.com/en/1.11/topics/db/transactions/) for how to use this.

## Commented Code
If the comment is providing useful documentation then it's fine. If it's old code, remove it.

## Handling Ajax Responses
Ajax requests can have the following:

1. Timeout with no response.
1. Success response.
1. Error response - Client (HTTP 400) or server error (HTTP 500).

All scenarios should be handled appropriately in javascript code.

# How Others Do It
Here are some example review policies from other companies:

1. [CockroachDB Style guide](https://github.com/cockroachdb/cockroach/blob/56cf78af266d25dba5192a94debb354275b01132/docs/style.md)
2. [Go Code Review Comments](https://github.com/golang/go/wiki/CodeReviewComments)
