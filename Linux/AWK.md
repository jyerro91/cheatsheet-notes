# AWK 


## Get total of a column
```bash
# Replace $1 with whatever column you wanted to total
sudo du -sh /path/* | awk -F " " '{Total=Total+$1} END{print "Total is: " Total}'    
```
