# opensmtpd_ldap_fail
Ldap Fails Unfortunately

###Setup###
````
vagrant up
vagrant ssh
cd /vagrant
make
````



Run **python tests/smtp_test.py**
`````
root@mail:opensmtpd_ldap_fail# python tests/smtp_test.py
Traceback (most recent call last):
  File "tests/smtp_test.py", line 30, in <module>
    smtp.login('test@arenstar.net', 'password')
  File "/usr/lib/python2.7/smtplib.py", line 610, in login
    AUTH_PLAIN + " " + encode_plain(user, password))
  File "/usr/lib/python2.7/smtplib.py", line 394, in docmd
    return self.getreply()
  File "/usr/lib/python2.7/smtplib.py", line 368, in getreply
    raise SMTPServerDisconnected("Connection unexpectedly closed")
smtplib.SMTPServerDisconnected: Connection unexpectedly closed
`````



**docker logs -f opensmtpd**
`````
info: OpenSMTPD 5.7.3p2 starting
info: startup
queue: queue compression enabled
queue: queue encryption enabled
debug: table-ldap: reading key "url" -> "ldap://ldap.arenstar.net"
debug: table-ldap: reading key "basedn" -> "dc=arenstar,dc=net"
debug: table-ldap: reading key "username" -> "cn=admin,dc=arenstar,dc=net"
debug: table-ldap: reading key "password" -> "password"
debug: table-ldap: reading key "domain_filter" -> "(&(objectClass=domain)(dc=%s))"
debug: table-ldap: parsing attribute "domain_attributes" (1) -> "dc"
debug: table-ldap: reading key "credentials_filter" -> "(&(objectClass=posixAccount)(uid=%s))"
debug: table-ldap: parsing attribute "credentials_attributes" (2) -> "uid,userPassword"
debug: table-ldap: reading key "userinfo_filter" -> "(&(objectClass=posixAccount)(uid=%s))"
debug: table-ldap: parsing attribute "userinfo_attributes" (3) -> "uid,uidNumber,gidNumber,homeDirectory"
debug: table-ldap: reading key "alias_filter" -> "(&(objectClass=nisMailAlias)(cn=%s))"
debug: table-ldap: parsing attribute "alias_attributes" (1) -> "rfc822MailMember"
debug: table-ldap: done reading config
debug: table-ldap: ldap server accepted credentials
debug: table-ldap: connected
debug: table-ldap: reading key "url" -> "ldap://ldap.arenstar.net"
debug: table-ldap: reading key "basedn" -> "dc=arenstar,dc=net"
debug: table-ldap: reading key "username" -> "cn=admin,dc=arenstar,dc=net"
debug: table-ldap: reading key "password" -> "password"
debug: table-ldap: reading key "domain_filter" -> "(&(objectClass=domain)(dc=%s))"
debug: table-ldap: parsing attribute "domain_attributes" (1) -> "dc"
debug: table-ldap: reading key "credentials_filter" -> "(&(objectClass=posixAccount)(uid=%s))"
debug: table-ldap: parsing attribute "credentials_attributes" (2) -> "uid,userPassword"
debug: table-ldap: reading key "userinfo_filter" -> "(&(objectClass=posixAccount)(uid=%s))"
debug: table-ldap: parsing attribute "userinfo_attributes" (3) -> "uid,uidNumber,gidNumber,homeDirectory"
debug: table-ldap: reading key "alias_filter" -> "(&(objectClass=nisMailAlias)(cn=%s))"
debug: table-ldap: parsing attribute "alias_attributes" (1) -> "rfc822MailMember"
debug: table-ldap: done reading config
debug: table-ldap: ldap server accepted credentials
debug: table-ldap: connected
smtp-in: session b9ed0d5b1b2d66d1: connection from host ldap.arenstar.net [172.17.0.1] established
debug: table_ldap: ldap_query: filter=(&(objectClass=domain)(dc=arenstar.net)), ret=1
smtp-in: session b9ed0d5b1b2d66d1: msgid=88d01c66, status=Ok, from=<contact@davidarena.net>, to=<test@arenstar.net>, size=341, ndest=1, proto=ESMTP
smtp-in: session b9ed0d5b1b2d66d1: connection from host ldap.arenstar.net [172.17.0.1] closed (client sent QUIT)
smtp-in: session b9ed0d6585831ba4: connection from host ldap.arenstar.net [172.17.0.1] established
smtp-out: session b9ed0d6826f9ff4c: connecting to lmtp://172.17.0.1:24 (ldap.arenstar.net)
smtp-in: session b9ed0d6585831ba4: TLS started version=TLSv1/SSLv3 (TLSv1.2), cipher=ECDHE-RSA-AES256-GCM-SHA384, bits=256
debug: table_ldap: ldap_query: filter=(&(objectClass=posixAccount)(uid=test@arenstar.net)), ret=1
warn: table-api: pipe closed
warn: parent -> lka: pipe closed
warn: control -> lka: pipe closed
warn: ca -> control: pipe closed
warn: scheduler -> control: pipe closed
`````

opensmtpd drops out without error
