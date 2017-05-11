# logrotate
The sample using script of logrotate

The nginx:
[root@bird ~]# cat /etc/logrotate.d/nginxlogrotate 
/var/log/server/nginx/*.log {
        compress
        notifempty
        daily
        dateext 
        rotate 30
        create
        sharedscripts
        postrotate
                /etc/init.d/nginx reload > /dev/null 2>/dev/null || true
        endscript
}

The tomcat using jsvc, 
refer to https://wiki.apache.org/tomcat/FAQ/Logging#Q10 and https://plonexp.leocorn.com/leocornus/leocornus.buildout.cfgrepo/xps35
( If you are using jsvc 1.0.4 or later (from Apache Commons Daemon project) to launch Tomcat, you can send SIGUSR1 signal to jsvc to get it to re-open its log files (Jira Ticket). You can couple this with 'logrotate' or your favorite log-rotation utility (including good-old 'mv') to re-name catalina.out at intervals and then get jsvc to re-open the original (catalina.out) file and continue writing to it. )
/path/to/tomcat/catalina/log.files {
    postscript
        /bin/kill -SIGUSR1 /path/to/jsvc.pid
    endscript
}
