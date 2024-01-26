# ctf_ceolost

##Problema



##Solução

Primeiro, é necessário que se decompile o arquivo jar dado: BankChallenge.jar

Utilizando-se de um decompilador online, ele nos gera 2 arquivos resultantes: Main.java e a.java

Dessa forma, quando abrimos esses resultados percebe-se que será necessário realizar uma limpeza:

ie. Transformando unicode -> utf16
```java
   public static void main(String[] var0) {
      System.out.println(b("챊챊챊챊챊챊챊챊챊챊챊챊챊챊챊챊챊챊챊챊챊챊챊챊챊챊챊챊챊챊"));
      System.out.println(b("찠찒찛찔찘찚찒챗찃찘챗찣찘찃찖찛찛찎찤찒찔찂찅찒찵찖찙찜\ued55"));
      System.out.println(b("챊챊챊챊챊챊챊챊챊챊챊챊챊챊챊챊챊챊챊챊챊챊챊챊챊챊챊챊챊챊"));
      System.out.println();
      System.exit(a());
   }
```
```java
    public static void main(String[] var0) {
        System.out.println(b(
                "\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a"));
        System.out.println(b(
                "\ucc20\ucc12\ucc1b\ucc14\ucc18\ucc1a\ucc12\ucc57\ucc03\ucc18\ucc57\ucc23\ucc18\ucc03\ucc16\ucc1b\ucc1b\ucc0e\ucc24\ucc12\ucc14\ucc02\ucc05\ucc12\ucc35\ucc16\ucc19\ucc1c\ued55"));
        System.out.println(b(
                "\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a\ucc4a"));
        System.out.println();
        System.exit(a());
    }
```
Ao fazer também algumas alterações no a.java e Main.java, pode-se forçar a saída da senha do admin:

![IMG1](https://github.com/dahiwas/ctf_ceolost/blob/main//im1.jpeg)

Após isso podemos reescrever método que realiza a cifração de forma simplificada:

```py
while i <= len(password):
    j = 0
    while j < len(dec):
        password[j] = ([password][j] + (i - 12) * j + 6) & 0xFF
        j += 1
    i += 1 

Base64.encode(password) -> return as UTF-16 format
```
Ou seja, para decifrar basta trocar os sinais '+' para '-':

```py
enc = "瑥⽦䩧㡡倰噕卖䝃捉㉌永敲畴楺癲湊".encode('utf-16be')
print(enc)
import base64
dec = list(base64.b64decode(enc.decode()))
var2 = 1
while var2 <= len(dec):
    var3 = 0
    while var3 < len(dec):
        dec[var3] = (dec[var3] - (var2 - 12) * var3 - 6) & 0xFF
        var3 += 1
    var2 += 1
print(f"uoftctf{{{''.join([chr(i) for i in dec])}}}")
```

Chegando então na flag desejada:

![IMG1](https://github.com/dahiwas/ctf_ceolost/blob/main//im2.jpeg)

Auxilio: https://github.com/jjchoNC/ctf-writeups/tree/main/UofTCTF/rev/CEO's%20Lost%20Password
