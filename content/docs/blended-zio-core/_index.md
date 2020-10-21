---
giturl: "https://github.com/blended-zio/blended-zio-core"
---
# blended-zio-core

Functionality that is required by all _blended_ containers.

## Configuring Blended Containers

As outlined [here]({{< relref "/docs/blended_container.md" >}}) all modules that require configuration should be able to use external configuration files containing place holders to specifiy lookups from environment variables or resolve encrypted values.

For example, the configuration for an LDAP service might be:

```
{
  url             : "ldaps://ldap.$[[env]].$[[country]]:4712"
  systemUser"     : "admin"
  systemPassword" : "$[(encrypted)[5c4e48e1920836f68f1abbaf60e9b026]]"
  userBase"       : "o=employee"
  userAttribute"  : "uid"
  groupBase"      : "ou=sib,ou=apps,o=global"
  groupAttribute" : "cn"
  groupSearch"    : "(member={0})"
}
```

The ZIO ecosystem has a library called [zio-config](https://zio.github.io/zio-config/) which supports different sources such as property files, HOCON, YAML or even the command line. At the core of the library are ConfigDescriptors which can be used to read the config information into config case classes. The descriptors are also used to generate documentation for the available config options or reports over the configuration values used within the application.

To provide support for string values, we introduce a `LazyConfigString` as follows:



## Simple crypto service

## Evaluate simple string expressions

## Define configuration objects
