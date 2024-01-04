# Analyzing Obfuscated Scripts - PowerShell

PowerShell, serving as the interpreter for the .NET framework, provides an expansive playground for those with malicious intent. Its ability to invoke deep and powerful classes within the .NET framework makes it a potent tool in the hands of malicious actors.

## Arm the Sample and Open it in Visual Studio Code
![image](https://github.com/CertainRisk/Analyzing-Obfuscated-Scripts-PS/assets/141761181/009bb3e0-8799-417c-976b-0354db012f59)


Malicious actors often resort to obfuscation techniques. Tactics include changing call variables and constructing lengthy strings. Since PowerShell is an extremely malleable language, these capabilities are amplified, enabling the language to be molded for various nefarious purposes.

### Invoke Exception (iEx) - A Powerful Shorthand

The `iEx` (invoke exception) is a shorthand in PowerShell that signifies executing whatever is inside its parentheses. For example:

```powershell
iEx(New-Object IO.Compression.DeflateStream
    ([IO.MemoryStream][Convert]::FromBase64String("BASE64_STRING_HERE")),
    [System.IO.Compression.CompressionMode]::Decompress)
```
Breaking it down:

- ![image](https://github.com/CertainRisk/Analyzing-Obfuscated-Scripts-PS/assets/141761181/1900cdd1-ee76-4e86-a214-68850c4bbac6)
 designates the creation of an object.
- ![image](https://github.com/CertainRisk/Analyzing-Obfuscated-Scripts-PS/assets/141761181/5992619b-8a7c-48fd-926a-4772e2713602)
 converts a base64 string (the big block of nonsense) into an IO.MemoryStream class, reading it into a memory stream.
- **System.IO.Compression.CompressionMode** calls the class, specifying decompression.
- Base64 string, converting that to the iO.memoRYStREam class that reads it into a memory stream. 
- **sYsTem.io.comprEsSIOn.CoMPReSsIONmOdE** - Is called and we are decompressing

So now we know we have a Base64 string that is currently compressed.
PowerShell is telling this to make an object that we can work with and decode the Base64 block and decompress it.
Then we pass it to the next part:
![image](https://github.com/CertainRisk/Analyzing-Obfuscated-Scripts-PS/assets/141761181/abd2ae73-4940-4e77-aedc-7c433ddff620)


## Variable Assignment for Defanging

To defang the script and allow it to do the work while assigning it to a variable, replace the `iEx` command with a variable assignment:
- We will defang it in this way but also make it do all the heavy lifting for us.
![image](https://github.com/CertainRisk/Analyzing-Obfuscated-Scripts-PS/assets/141761181/71d433f0-7244-4dd2-95eb-c650b115f00b)

In order to do this we delete the "iEx(" and the last ")" and then assign this to a variable. 

We then invoke the variable in PowerShell via Cmder with the $.
![image](https://github.com/CertainRisk/Analyzing-Obfuscated-Scripts-PS/assets/141761181/e1fd2c46-9065-4c72-9ff5-7ab287b7d029)

```Powershell
$decodedScript = New-Object IO.Compression.DeflateStream
    ([IO.MemoryStream][Convert]::FromBase64String("BASE64_STRING_HERE")),
    [System.IO.Compression.CompressionMode]::Decompress
```
Now, the decoded script is stored in the `$decodedScript` variable.
If no errors occur, your script is good to go!
![image](https://github.com/CertainRisk/Analyzing-Obfuscated-Scripts-PS/assets/141761181/a8d23461-4297-4c73-aa1d-6fdce586c454)

## A Simple PowerShell Reverse TCP Shell

![image](https://github.com/CertainRisk/Analyzing-Obfuscated-Scripts-PS/assets/141761181/3f83c15c-17c3-4681-ad35-c5992e9a21c3)














