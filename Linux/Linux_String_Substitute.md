There is some data on Nautilus App Server 2 in Stratos DC. Data needs to be altered in several of the files. On Nautilus App Server 2, alter the /home/BSD.txt file as per details given below:  
a. Delete all lines containing word code and save results in /home/BSD_DELETE.txt file. (Please be aware of case sensitivity)  
b. Replace all occurrence of word and to them and save results in /home/BSD_REPLACE.txt file.  
Note: Let's say you are asked to replace word to with from. In that case, make sure not to alter any words containing the string itself; for example upto, contributor etc.  

Solution:
```
sed '/code/d' /home/BSD.txt > /home/BSD_DELETE.txt
sed 's/and/them/g' /home/BSD.txt > /home/BSD_REPLACE.txt
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)  
