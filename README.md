# metasploit_vbs_base64decode_trouble

I encountered a minor issue when using base64 decoding to transmit shellcode

# Process

![data](https://github.com/thedarknessdied/metasploit_vbs_base64decode_trouble/blob/main/data.png)

I am preparing to reproduce the tomcat_cgi_cmdlineargs vulnerability using MSF. I have set up a vulnerability environment locally and successfully executed commands through manual POC verification. However, when I used MSF for verification, I selected windows/x64/meterpreter_reverse_tcp as the attack payload, but I was unable to correctly obtain a reverse shell

![resource.png](https://github.com/thedarknessdied/metasploit_vbs_base64decode_trouble/blob/main/resource.png)

I found that the encoded data is decoded to base67 using a VBS script here, but I cannot obtain a shell back connection. 

![vbs.png](https://github.com/thedarknessdied/metasploit_vbs_base64decode_trouble/blob/main/vbs.png)

I used a Python script for decoding for comparative testing. My decoding code is as follows:

```python
import base64


filename = r"pdQci.b64"

with open(filename, "rb") as f:
    content = f.read()
    data = base64.b64decode(content)
    with open(r"pdQci.exe", "wb") as g:
        g.write(data)
```

These are files obtained through different script decodings. The file sizes are the same, but the file obtained through VBS decoding cannot be executed, while the file obtained through Python decoding can be successfully executed.

![size.png](https://github.com/thedarknessdied/metasploit_vbs_base64decode_trouble/blob/main/size.png)

I compared the decoded file with WinHex and found many different bytecodes
python:
![python_decode](https://github.com/thedarknessdied/metasploit_vbs_base64decode_trouble/blob/main/python_decode.png)
vbs:
![vbs_decode.png](https://github.com/thedarknessdied/metasploit_vbs_base64decode_trouble/blob/main/vbs_decode.png)
