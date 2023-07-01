# Emoji
> My friend sent a message that contains a lot of emojis. He says that there is a secret message behind it.

> Can you find the message behind it?

> Format Flag: ForestyHC{DECRYPTEDMESSAGE}

> Author: bleedz#0666 (Discord) / @bl33dz (Tele)

Hint : 
```
import string

def emojiCrypt(plainText):
    emojis = []
    for c in plainText:
        if c not in string.ascii_uppercase:
            emojis.append(c)
        else:
            code = string.ascii_uppercase.index(c) + 0x1F600
            emojis.append(chr(code))
    return ' '.join(emojis)
```
# About the Challenge
Diberikan sebuah file message.txt yang berisikan beberapa emoji
```
😈 😒 😓 😇 😈 😒 😑 😄 😀 😋 😋 😘 😂 😑 😘 😏 😓 😎 😂 😇 😀 😋 😋
```
# How To Solve
Jika kita baca code yang diberikan pada hint, disitu bisa kita lihat kalau code tersebut merubah setiap text yang diberikan menjadi uppercase dan merubahnya menjadi Emoji, cara solve nya berarti tinggal dibalik saja logika nya yaitu dari emoji ke text.

Disini saya sudah membuat code untuk solve nya
```
emojis = ['😈', '😒', '😓', '😇', '😈', '😒', '😑', '😄', '😀', '😋', '😋',
          '😘', '😂', '😑', '😘', '😏', '😓', '😎', '😂', '😇', '😀', '😋', '😋']

emoji_map = {
    '😀': 'A', '😁': 'B', '😂': 'C', '😃': 'D', '😄': 'E', '😅': 'F', '😆': 'G',
    '😇': 'H', '😈': 'I', '😉': 'J', '😊': 'K', '😋': 'L', '😌': 'M', '😍': 'N',
    '😎': 'O', '😏': 'P', '😐': 'Q', '😑': 'R', '😒': 'S', '😓': 'T', '😔': 'U',
    '😕': 'V', '😖': 'W', '😗': 'X', '😘': 'Y', '😙': 'Z'
}

word = [emoji_map.get(i, '') for i in emojis]
result = 'ForestyHC{' + ''.join(word) + '}'
print(result)
```

Dan didapatkan Flag nya yaitu : ForestyHC{ISTHISREALLYCRYPTOCHALL}
