# Santa's Forensics 101
X-MAS -> Forensic
## Tools
Tool | Description
------- | -------
binwalk |

## Description

We just need to extract with binwalk and grep for the flag:

```
binwalk -e X-MAS_Flag2.png
cd _X-MAS_Flag2.png.extracted
cd hidden_data_dt
cat logo2.png | grep "X-MAS"
```

## FLAG
***X-MAS{W3lc0m3_t0_th3_N0rth_Pol3}***
