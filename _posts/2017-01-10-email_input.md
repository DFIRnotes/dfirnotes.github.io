_Notes on email-based file submission to analysis platforms_

##Mail

*mailutils*

Send a file attachment.
``` mail -s'bark bark issue' adric@marie < /etc/issue ```

Proved with mutt the message and file get mime-encoded and attached, replied with more files, works.

*Postfix*

https://serverfault.com/questions/258469/how-to-configure-postfix-to-pipe-all-incoming-email-to-a-script

http://www.postfix.org/VIRTUAL_README.html#mailing_lists

*Procmail*

http://tldp.org/LDP/LG/issue14/procmail.html

##mastiff

```$ mas.py /path/to/file2analyze```

##Viper
Viper API docs, show how to use REST endpoint and curl for file submission

```$ curl -F file=@FILE -F tags='foo,bar' -X POST http://127.0.0.1:8080/file/add```

##crits

Probably via services, or will need to integrate with them for ingest processing

* https://hub.docker.com/r/auxsec/crits_services/
* https://github.com/crits/crits_services
* https://github.com/crits/crits/wiki/Service%20API

And Mongo is likely involved, as SQLite is for Mastiff and Viper

```
sudo docker run --name docker_mongo -p 27017:27017 -v /cases/crits-data:/data/db -d mongo:latest --auth
sudo docker run --name crits --link docker_mongo:mongo -p 8443:8443 -e MONGO_USER=crits -e MONGO_PASSWORD=password -d auxsec/crits_services
```
