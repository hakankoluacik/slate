# Errors

The SafeLock API uses the following error codes:


Error Code | Meaning
---------- | -------
401 | Unauthorized -- Your API key (api_token) is wrong.
404 | Not Found -- The specified resource could not be found.
405 | Method Not Allowed -- You tried to access a resource with an invalid method.

<aside class="notice">
Each error will come with <b>"status": "fail"</b>
</aside>
