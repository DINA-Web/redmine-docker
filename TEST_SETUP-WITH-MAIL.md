# pre-req for testing:
A. mail-docker is running ( run -> $docker network ls and check for the network called 'redminedocker_default'  ) 
B. redmine-docker is running ( network 'redminedocker_default' ($docker network ls) ) 
Test: pull the network-cable, log in to redmine-docker and ping 'mail.dina-web.net
Does it work? : YES!

## you have mail-docker running i prod (wrangler)

## Step 1: Focus mail

A.1 Create 2 email-accounts (for instance iceman@mail.dina-web.net  and akranes@mail.dina-web.net)
-- see Makefile on how to create (see the target 'email_add-account')
A-2 List email-accounts
-- see Makefile on how to list (make email_list-accounts)
A.2 check that there is No network-connection
A.3 Set up the 2 accounts in thunderbird (me ->using the Lenovo-computer)
A.4 Test to send a mail from iceman@mail.dina-web.net to akranes@mail.dina-web.net, simple subject like '1234'
Does it work  work : YES!

## Step 2: Focus redmine

B.1 login to https://support.dina-web.net
B.2 create the project 'myproject' 
-- 'myproject' corresponds to the email-address (see the file '/usr/src/redmine/mail-script/receive_imap.sh')
B.3 Add the user 'akranes@mail.dina-web.net'
B.3 login to the redmine-docker-'container' ( docker exec -it dina_redmine bash )
B.4 run 'crontab -l' (start the cron job if it is not running)
-- verify the line : "* * * * * /bin/bash -l -c '/usr/src/redmine/mail-script/receive_imap.sh' " 
B.5 *Nota-Bene , for production* the script receive_imap.sh generates the file '/tmp/cron-mail.log' ( see : --trace >> /tmp/cron-mail.log 2>&1 ) - remove that log file in production
b.6 run the script manually : /usr/src/redmine/mail-script/receive_imap.sh
-- verify-1: cat /tmp/cron-mail.log - Errors ?
-- verify-2: check the mailbox of i.e iceman@mail.dina-web.net, is the mail gone ?
-- verify-3: was there an Issue created in project 'myproject' ?

check if cron is running.
# /etc/init.d/cron status
if you get this reply "[FAIL] cron is not running ... failed! " then you need to start the cron-job
# /etc/init.d/cron start 
[ ok ] Starting periodic command scheduler: cron.
and check again 
# /etc/init.d/cron status
[ ok ] cron is running. (should be the reply


@ToDo.
now mails need to have the 'mail' after the @-sign, use alias to get rid of ?
