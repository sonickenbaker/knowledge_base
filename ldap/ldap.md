# LDAP

## Perform a search (no line wrapping)
```
ldapsearch -o ldif-wrap=no -x -W -D "<bind_dn>" -b "<base_dn>" [attributes...]

-o <opt>[=<optparam>] general options
           nettimeout=<timeout> (in seconds, or "none" or "max")
           ldif-wrap=<width> (in columns, or "no" for no wrapping)
-W         prompt for bind password
-x         Simple authentication
-D binddn  bind DN
-b basedn  base dn for search
```

## Search and decode users password
```
ldapsearch -o ldif-wrap=no -x -W -D "<bind_dn>" -b "<base_dn>" userPassword | grep userPassword | grep -oP '(?!userPassword:: )\S+$' | base64 -d -
```

## Find the rootDN (that you can use as a bind dn to perform operations)
```
sudo ldapsearch -H ldapi:// -LLL -Q -Y EXTERNAL -b "cn=config" "(olcRootDN=*)" dn olcRootDN olcRootPW
```

