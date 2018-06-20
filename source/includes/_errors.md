# Errors

The Canonn APIv2 uses the following error codes:

Error Code | Meaning
---------- | -------
400 | Bad Request -- Your request sucks
401 | Unauthorized -- Your JSON-Web-Token is wrong
403 | Forbidden -- Hey! Your not supposed to be here!
404 | Not Found -- Yup I think the Thargoids stole this data
405 | Method Not Allowed -- Error, does not compute
406 | Not Acceptable -- You requested a format that isn't json
410 | Gone -- The requested record has been removed from our servers
418 | I'm a ~~teapot~~ Tharoid
429 | Too Many Requests -- You're requesting too many artifacts! Slow down!
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarially offline for maintanance. Please try again later.
