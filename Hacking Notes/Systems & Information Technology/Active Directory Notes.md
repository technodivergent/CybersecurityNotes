# Domain Controller
- Hosts AD Domain Service data
	- Ntds.dit file contains all data: users, groups, objects, etc
	- Storage location: `%SystemRoot%\NTDS`
	- Accessible only thru DC processes/protocols
	- Can use to steal hashes
- Provides authn/authz
- Replicate updates to other DC in domain/forest
- Allow admin access to manage user accts & network

# AD DS Schema
- Defines every type of object that can be stored in directory
- Enforces rules regarding obj creation & configuration

| Object Types     | Function                                      | Examples        |
| ---------------- | --------------------------------------------- | --------------- |
| Class Object     | What objects can be created in the directory  | User / Computer |
| Attribute Object | Information that can be attached to an object | Display name    |

# Domain
- Administrative boundary for applying policies to groups of objects in an organization
- Replication boundary for replicating data between DCs
- Authn/authz boundary that provides a way to limit the scope of access to resources
# Trees
- Group of domains, e.g.:
	- contoso.com
		- emea.contoso.com
		- na.contoso.com
- Shares contiguous namespace with parent domain
- Creates two-way transitive trust with other domains

# Forest
- Collection of one or more domain trees
- Share a common
	- Schema
	- Configuration partition
	- Global catalog to enable searching
- Enables trusts between all domains in the forest
- Shares Enterprise Admins and Schema Admins groups

# Organization Units
- AD containers for users, groups, computers & other OUs
- Represent org hierarchically and logically
- Manage collection of objects in consistent way
- Helps delegate permissions to administer groups of objects
- Apply policies

# Trusts
- Provide a mechanism for users to gain access to resources in another domain
- All domains in a forest trust all other domains in the forest
- Trusts can extend outside the forest

![[Pasted image 20230407212330.png]]

