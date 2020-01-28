# Overview
Using curl is useful for testing api calls. However, using it when an api requires authentication or csrf tokens can pose a challenge. One way to handle this is to login via a web browser, viewing the request and response using a browser debugger, e.g. Chrome Developer Tools. You can use the debugger to extract HTTP information required to enable curl to work.

# Example

Here's an example curl command using a parameter file:
```
curl -i -K params.txt http://127.0.0.1/api/some-function/
```

The contents of _params.txt_:
```
-X POST
-H "Host: demo.localhost"
-H "Content-Type: application/json"
-H "Authorization:JWT eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6Impvc2VwaGNvbnN0QHlvcG1haWwuY29tIiwidXNlcl9pZCI6NSwiZW1haWwiOiJqb3NlcGhjb25zdEB5b3BtYWlsLmNvbSIsImV4cCI6MTUwODg3NDA3MX0.scrK27gxi_GlnV37VQ9gSX1fzUVkccHCDero6b-8aJ0"
-H "Cookie:csrftoken=PQ34my0xMgX22KctWeifp5SYpY4biJ1D; sessionid=xp5xrsupdibljxu1td0ak2ocluarjnd0;"
-H "X-CSRFToken:PQ34my0xMgX22KctWeifp5SYpY4biJ1D"
```
The JWT and CSRF headers are extracted from the browser tool.
