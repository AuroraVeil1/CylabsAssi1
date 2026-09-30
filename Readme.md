# <center><b>Assignment 1 writeup</b></center>

# RED (forensics)	

## Approach
* Metadata + Steganography
* Tools used: WSL and [CacheSleuth](https://www.cachesleuth.com/tools/steganography/)

## Solution
* My first step in any forensics challenge is to check the metadata of the image using ```exiftool image.png```
* The metadata provided a poem in which every capitalized letter spelled out "CHECK LSB"
* ![The metadata provided a poem in which every capitalized letter spelled out "CHECK LSB"](<img width="1457" height="77" alt="image" src="https://github.com/user-attachments/assets/abe7cae2-f1bb-4dc9-84c7-acd82af7aacd" />

* Although I didn't know what LSB was, I still googled "Check lsb of image" and got this website
* The second hint, ```Red?Ged?Bed?Aed?``` gave me an idea of which checkboxes to click (RGBA)
* Finally, the output provided me with a base64 string which I decoded using cyberchef and got the flag

## Flag
academy{r3d_1s_th3_ult1m4t3_cur3_f0r_54dn355_}

## Takeaway
There isn't always a need to install new programs for steganography...most of them are available on the internet directly

# CanYouSee (forensics)

## Approach
* Metadata analysis
* Tools used: WSL (exiftool) and CyberChef

## Solution
* As with any forensics challenge, I started by checking the metadata of the image using ```exiftool image.jpg```
* <img width="1027" height="33" alt="image" src="https://github.com/user-attachments/assets/5fb8fea5-f8f1-4ec7-923e-7c1e50b84608" />

* This was an easier version of the RED challenge, since the metadata directly contained a base64 string
* I pasted the string into CyberChef, used the "From Base64" operation and got the flag

## Flag 
academy{ME74D47A_HIDD3N_aebe8c0f}

## Takeaway
Always check the metadata first, sometimes the answer is hiding in plain sight


# EVEN RSA CAN BE BROKEN??? (crypto)

## Approach
* RSA decryption using known prime factors
* Tools used: [dCode RSA Cipher](https://www.dcode.fr/rsa-cipher)

## Solution
* I knew RSA was a cipher, so I went straight to dCode's RSA decoder
* I entered the values of C, E, N, P and Q provided in the challenge
* <img width="555" height="527" alt="image" src="https://github.com/user-attachments/assets/9ad50231-826b-44ac-9687-6d7ad710cf6c" />

* Since P and Q were known, dCode computed the private key D by itself and decrypted the message, giving me the flag directly
*<img width="427" height="122" alt="image" src="https://github.com/user-attachments/assets/2b2dbd48-1d38-4818-b013-ddaac020ce9c" />

* Because the values were already given, I did not need to analyze the provided source code

## Flag
academy{tw0_1$_pr!m39cf1fb4f}

## Takeaway
Dedicated online tools like dCode can solve many crypto challenges instantly, so it is worth checking them before writing any custom code

# vault-door-training (reverse engineering)

## Approach
* Source code analysis
* Tools used: Visual Studio Code

## Solution
* I opened the provided Java source code in VS Code and read through it
* The password was written directly in the code as a plain string, `w4rm1ng_Up_w1tH_jAv4_69bd4dc7adf`
* I wrapped it in the flag format, academy{...}, and that gave me the flag

## Flag
academy{w4rm1ng_Up_w1tH_jAv4_69bd4dc7adf}

## Takeaway
 Always analyze the provided code in VS Code first, the answer can be sitting right there in the source

# Transformation (reverse engineering)

## Approach
* (TOOK HELP FROM MY FRIEND SINCE I DIDN'T KNOW A THING ABOUT PYTHON)
The challenge description contains code that takes each pair of 2 characters from the flag, and replaces them with a single character with the UTF code `char_1 * 256 + char_2`. Since the UTF codes for characters in the English alphabet are less than 256, `encoded_flag[i] // 256` and `encoded_flag[i] % 256` should give us char_1 and char_2 back.

## Solution
* I opened the `enc` file and saw a string of random-looking Chinese characters
* From the code in the description, `ord(flag[i]) << 8` is the same as multiplying by 256, so each character in the file holds two flag characters packed together
* To reverse it, I took the code of each character in the file and split it into two parts: `code >> 8` gives the first character and `code & 255` gives the second
* I did this with a short Python script:
```python
enc = open("enc", encoding="utf-8").read()
print("".join(chr(ord(c) >> 8) + chr(ord(c) & 255) for c in enc))
```

## Flag
academy{16_bits_inst34d_of_8_790ba37e}

## Takeaway
When a challenge packs two 8-bit characters into one 16-bit character, the bit shift is easy to reverse. Just split the value back into its high and low bytes.
