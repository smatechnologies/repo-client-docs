---
slug: '/'
sidebar_label: 'Servers'
---

# Servers

A server json definition file provides the information required by the RepoClient to connect to the target OpCon system. It includes a name, the address, port number and the OpCon user required to login into the OpCOn environment. Optionally it includes a system prefix which is used during workflow transformation.

```
{
  "name":"",
  "address":"",
  "port":,
  "user":"",
  "password":"",
  "systemPrefix":""
}

```
The information above provides a json template for creating the OpCon system json file.

attribute            | Value
-------------------- | -----------
**name**             | The name of the OpCon system and should match the name of the json file.
**address**          | The address of the ImpEx2 system (DNS name).
**port**             | An integer value defining the port number of the ImpEx2 Rest API server (default value is 9011)
**user**             | An OpCon defined user that has the required privileges to access the OpCon definitions.
**password**         | The password of the OpCon user encrypted using the Encrypt.exe utility.
**systemPrefix**     | An optional field that will automatically prefix definitions with this value to create unique workflow definitions. If not required leave this out.

Server definitions need to be created manually and then uploaded and committed into the **servers** branch of the **OPCON_SERVERS** repositories.