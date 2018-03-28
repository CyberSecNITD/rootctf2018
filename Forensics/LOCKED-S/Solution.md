The description of the problem is clear we need to find out what is the android pattern lock key combination. After extracting the compressed dump we get ```userdata-qemu.img``` which is to be mounted so that we can analyse it. 
# ```sudo mkdir /mnt/dump```
# ```sudo mount -o loop userdata-qemu.img /mnt/dump/```
After mounting the system we need to find a file named ```gesture.key``` because that's the file where pattern hash is stored on android.
# ```cd /mnt/dump/ | sudo find . -name 'gesture.key'```
Follow the path where it is located and try to see it's content using ```cat``` command . Weird characters ?? (XD) . Get it's Hexdump .
# ```cat gesture.key | xxd -p```
Got a hash ?? According to the documentation it is SHA-1 , due to fact that we have very finite possible pattern combinations and the other fact that Android OS does not use a salted hash, it does not take a lot to generate a dictionary containing all possible hashes of sequences from 0123 to 876543210.
Luckily i found one dictionary:- https://mega.nz/#!pA9xkRTa!WwjR4ssaTPp7fxbRHOwbqtj8M3JGLZhl4BCzKfq0wFk . It's a RainbowTables DB so compare the generated hash with hashes stored in this DB and get the desired pattern.
```
sqlite3 GestureRainbowTable.db
select * from RainBowTable where hash = 'Paste Your Hash Here';
```
Use this pattern to unlock the zip file and get the flag XD !!
