# Errors

The Sendrater API uses the following error codes:


Error Code | Meaning
---------- | -------
400 | Bad Request -- Your request is invalid.
401 | Unauthorized -- Your API key is wrong.
403 | Forbidden -- Administrators only.
404 | Not Found -- The specified endpoint could not be found.
405 | Method Not Allowed -- Please check your request method (GET/POST)
406 | Not Acceptable -- You requested a format that isn't allowed.
429 | Too Many Requests -- You're requesting more than rate limits.
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarily offline for maintenance. Please try again later.
