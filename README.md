C:\Users\ankit\zephyrproject\.venv\Scripts\Activate.ps1


west init -m https://github.com/Ankit792/minimil.git --mr main
west init -m git@github.com:Ankit792/minimil.git --mr main

west build -p always -b minimil_b1_dk/ab1c1f4m51820hh0/rtss_he minimil/samples/lc3/lc3_codec

west boards | findstr minimil
west boards --board minimil_b1_dk
west boards | findstr alif
west list

west build -p always -b minimil_b1_dk/ab1c1f4m51820hh0/rtss_he minimil/samples/lc3/lc3_codec
west build -p always -b minimil_b1_dk/ab1c1f4m51820ph0/rtss_he minimil/samples/lc3/lc3_codec
