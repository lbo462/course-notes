# CSC Course

[Course on GitHub](https://github.com/cunchem/csc)

## TD 2 JDR 

[Subject](https://github.com/cunchem/csc/blob/master/td2-jdr.md)

#### Pythonic solution

```python
import math

ALPHABET = "abcdefghijklmnopqrstuvwxyz "

p = ...  # REPLACE HERE WITH A PRIME NUMBER
q = ... # REPLACE HERE WITH A PRIME NUMBER

n = p * q
phi = (p - 1) * (q - 1)

e = [i for i in range(2, phi - 1) if math.gcd(i, phi) == 1][0]
d = pow(e, -1, phi)

public_key = (e, n)
private_key = (d, n)

print(f"[Public ] (e, n) = {public_key}\n[Private] (d, n) = {private_key}")


def crypt(message, key):
  uncrypted = "".join([f"{ALPHABET.index(x)+1:02}" for x in [*message]])
  blocs = [f"{uncrypted[i:i+3]:03}" for i in range(0, len(uncrypted), 3)]
  crypted_array = [str(pow(int(i), key[0], key[1])) for i in blocs]
  return "-".join(crypted_array)

def decrypt(message, key):
  d = "".join([f"{pow(int(i), key[0], key[1]):03}" for i in message.split("-")])
  m = [int(d[i:i+2]) for i in range(0, len(d), 2)]
  return "".join([ALPHABET[i-1] for i in m])
```

## TD 3 passwords

[Subject](https://github.com/cunchem/csc/blob/master/td3-passwords.md)

#### Solution :

`$ git diff`
```diff
diff --git a/td3-code/skeleton.py b/td3-code/skeleton.py
index a28895c..6fe1144 100755
--- a/td3-code/skeleton.py
+++ b/td3-code/skeleton.py
@@ -21,7 +21,7 @@ def crackencrypted(database):
     for i in database:
         # i[0] is the login, i[1] is the encrypted password
         #...
-        crackeddb.append((i[0],i[1])) # second argument should contain cleartext password
+        crackeddb.append((i[0], decrypt(key, i[1]))) # second argument should contain cleartext password
     return crackeddb


@@ -30,12 +30,12 @@ def crackencrypted(database):
 def cracksha(database):
     global nbpasswords
     passwords = getPassDict(nbpasswords) # passwords contains a dictionary of passwords
-    #...
+    hashes = genshahashes(passwords)
     crackeddb = []
     for i in database:
         # i[0] is the login, i[1] is the hashed password
         #...
-        crackeddb.append((i[0],i[1])) # second argument should contain cleartext password
+        crackeddb.append((i[0],getpassfromshahash(hashes, i[1]))) # second argument should contain cleartext password
     return crackeddb
 # Hint : salthash(password, salt) return the salted hash of password
@@ -44,9 +44,9 @@ def cracksaltedsha(database):
     passwords = getPassDict(nbpasswords)
     crackeddb = []
     for i in database:
-        # i[0] is the login, i[1] is the hashed password, i[2] is the salt
-        #...
-        crackeddb.append((i[0],i[1])) # second argument should contain cleartext password
+        for p in passwords:
+            if i[1] == salthash(p, i[2]):
+                crackeddb.append((i[0],p)) # second argument should contain cleartext password
     return crackeddb

 # Hint : pbkdf2(password, salt, nbiterations) returns the pbkdf2 of password using salt and nbiterations
@@ -55,9 +55,9 @@ def crackpbkdf2(database):
     passwords = getPassDict(nbpasswords)
     crackeddb = []
     for i in database:
-        # i[0] is the login, i[1] is the hashed password, i[2] is the salt, i[3] is the iteration count
-        #...
-        crackeddb.append((i[0],i[1])) # second argument should contain cleartext password
+        for p in passwords:
+            if i[1] == pbkdf2(p, i[2], i[3]):
+                crackeddb.append((i[0],p)) # second argument should contain cleartext password
     return crackeddb
```