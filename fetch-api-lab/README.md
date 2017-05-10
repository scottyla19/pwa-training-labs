# Notes
- Why didn't a failed response activate the catch block? This is an important note for fetch and promises—**bad responses (like 404s) still resolve!** A fetch promise only rejects if the request was unable to complete, so you must always check the validity of the response.
- Getting headers involves getting a headers object `var myHeaders = response.headers` then calling the get funciotn on
the object. `myHeaders.get(name)`
