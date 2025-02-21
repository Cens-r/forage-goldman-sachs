# forage-goldman-sachs
Job simulation for Goldman Sachs with the goal to crack a password database leak example.

## Steps Taken
1) Checked the hash type using an online [hash identifier](https://hashes.com/en/tools/hash_identifier)
2) Installed and setup [HashCat](https://github.com/hashcat/hashcat)
3) Downloaded the top 100k most used passwords from [ncsc.gov](https://www.ncsc.gov.uk/static-assets/documents/PwnedPasswordsTop100k.txt), using this command:
`curl -o "top-100k.txt" "https://www.ncsc.gov.uk/static-assets/documents/PwnedPasswordsTop100k.txt"`
4) Used HashCat to crack as many of the passwords as it could, using the word list from the previous step. The command for this was: `hashcat -m 0 -a 0 --username "password-dump.txt" "top-100k.txt"`
5) Dumped the list of successfully cracked passwords to a new file, using this command: `hashcat -m 0 --username --show "password-dump.txt" > "password-cracked.txt"`
6) Analyzed the cracked passwords and hashing technique using provided [questions](#Questions)
7) Drafted a [memo](security-memo.txt) to the Risk Management department at Goldman Sachs to convey security risks with their current password policy

## Questions
### What type of hashing algorithm was used to protect passwords?
Using an online [hash identifier](https://hashes.com/en/tools/hash_identifier), I was able to determine that all of the [password hashes](password-dump.txt) were very likely hashed with the MD5 algorithm.

### What level of protection does the mechanism offer for passwords?
The MD5 algorithm provides fairly weak protection compared to more modern hashing alternatives. It's better than not hashing at all, but there are some pretty major security risks. MD5 is a relatively fast hashing algorithm which makes it more open to brute-force attacks. Two or more inputs into the algorithm can produce the same hash output, making it vulnerable to collision attacks as well.

### What controls could be implemented to make cracking much harder for the hacker in the event of a password database leaking again?
A stronger hashing algorithm could be chosen instead of MD5, to make the hashing take longer and offer more security. Salt(s) could also be introduced to prevent the use of rainbow table attacks, and could be hardcoded or stored in a different database. If the password database was leaked the hackers would also need access to these salts.

### What can you tell about the organization’s password policy (e.g. password length, key space, etc.)?
From the passwords I was able to crack, the password policy is pretty weak. The longest passwords were nine characters long, and the shortest were six characters long. Usually a secure password is at the very minimum eight characters long. Noticeably it doesn't seem like numbers or special characters are required, as one of the passwords is "qwerty".

### What would you change in the password policy to make breaking the passwords harder? 
To make breaking the passwords harder, the minimum password length should be increased to eight characters and at least two numbers and one special character should be required. This would make passwords more unique and harder to crack.