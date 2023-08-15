POSTMORTEM Task


# Issue Summary
16/02/2022 From 9:10 AM to 10:30 AM UTC+1 all requests for the homepa
ge to our servers got a 404 response
# Timeline
- 9:10 AM : Glitch happened
-9:12 AM: Update pushed
- 9:11 AM : Noticing the problem
- 9:15 AM : Notifying all team members , front end and back end
- 9:20 AM : Successful change rollback
- 9:24 AM : Server Restarts begin
- 9:27 AM : 100% of traffic back online
- 9:30 AM : start debugging the push with the problem
- 9:50 AM : Problem fixed and pushed the changes
- 9:55 AM : Server restart begins
- 10:30 AM : 100% traffic back online with the new updates
# Root cause and resolution
After rolling back changes we knew that the changes were made by the
front end team so we took the broken changes and run them on a test s
server which replicated same problem, our server uses apache2 and apache2 error logs didn't give enough information about the problem so we
 traced the apache2 process using strace and when a request is sent s
trace tool catchs a lot of error and after some scaning fo these erro
rs we found the error which is a typo in page file extension >
> .phpp
instead of
> .php
and to fix that we just search in our main directory using grep for t
hat typo
> grep -inR ".phpp" .
after fixing the error we pushed back the changes and restarted the s
ervers
- 10:00 AM : 100% of traffic back online with the new updates
# Corrective and preventative measures
To prevent similar problems from happening again we will
- Create an automated test pipeline for every update push
- Add a monitoring software to our servers which will monitor lot of
things and one of them Network Traffic requests and responses and co
figure it to notify  the teams when too much non desired res
ponses were sent like 404
- Create a tests for every new update and the teams should not push
until those tests pass
~
~

