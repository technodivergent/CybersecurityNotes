Install Win Svr 2k19
Rename PC
Add AD role
Promote to DC
Add AD cert svcs
Configure cert svcs
Add users
Add service account
```
cmd.exe as admin:
setspn -a WINDC/SQLService.NEWEDEN.local:60111 NEWEDEN\SQLService 
setspn -T NEWEDEN.local -q */*
```
- 
- 
- 