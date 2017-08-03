Configure HTTP server: Apache HTTP Server
=========================================

Requirements
------------

* [Apache HTTP Server](http://httpd.apache.org/download.cgi).

* [mod_fcgid](http://httpd.apache.org/mod_fcgid/), FastCGI server for Apache.
  Note that "mod_fastcgi" is no longer maintained and highly discouraged.

* [FCGI](https://metacpan.org/release/FCGI), FastCGI interface for Perl.

General instruction
-------------------

1. Add following excerpt to httpd configuration and edit it as you prefer
   (Note: replace [``$LIBEXECDIR``](../layout.md#libexecdir) and
   [``$STATICDIR``](../layout.md#staticdir)):
   ```
   ### Apache Configuration for Sympa

   ## Definition of Sympa FastCGI server.
   <IfModule mod_fcgid.c>
       IPCCommTimeout 300
       # You wish to increase the value in following line, don't you?
       MaxProcessCount 5
       MaxRequestLen 131072

       # Don't forget to edit this section!
       <Location /sympa>
           SetHandler fcgid-script

           # Don't forget to edit lines below!
           Order deny,allow
           Deny from all
           #Allow from all
       </Location>
       ScriptAlias /sympa $LIBEXECDIR/wwsympa-wrapper.fcgi

   #    # You may uncomment following lines to enable SympaSOAP feature.
   #    <Location /sympasoap>
   #        SetHandler fcgid-script
   #
   #        # Don't forget to edit lines below!
   #        Order deny,allow
   #        Deny from all
   #        #Allow from all
   #    </Location>
   #    ScriptAlias /sympasoap $LIBEXECDIR/sympa_soap_server-wrapper.fcgi
   </IfModule>

   ## Other static contents
   Alias /static-sympa $STATICDIR

   ## If your host is dedicated to Sympa:
   #RewriteEngine on
   #RewriteRule ^/?$ /sympa [R=301]
   ```

2. Restart httpd.
