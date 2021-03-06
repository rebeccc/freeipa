# Ticket Lifetime Jitter

## Overview

Ticket lifetimes can be jittered so that renewals / re-issues do not overwhelm the KDC at a certain moment. 
The feature is enabled automatically so we can avoid triggering an LDAP query on every AS_REQ and TGS_REQ. 

## Use case

As Administrator of a cluster, I want to configure the IdM Server KDC to issue TGTs with lifetime jitter, so that my entire cluster does not renew Kerberos tickets at the same time, causing overload of IdM Server KDC.

## Implementation 

Instead of a static 24 hour TGT lifetime, KDC does (23 hour + rand[0, 60] minute) lifetime. If an authentication method is specified that has a non-24hour lifetime, KDC does (lifetime - 1 hour + rand[0, 60] minute). If the lifetime is already set to 60 minutes or less the lifetime will not be jittered. 
