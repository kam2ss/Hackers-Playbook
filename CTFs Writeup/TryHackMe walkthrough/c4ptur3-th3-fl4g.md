#### Translation & Shifting
1. **c4n y0u c4p7u23 7h3 f149?**
This text is substituting letters to number, we can decipher it by substituting the numbers back to letters.
Answer: `can you capture the flag?`

2. `01101100 01100101 01110100 01110011 00100000 01110100 01110010 01111001 00100000 01110011 01101111 01101101 01100101 00100000 01100010 01101001 01101110 01100001 01110010 01111001 00100000 01101111 01110101 01110100 00100001`
This seems to be a binary number. So, using `cyberchef` and choosing `From Binary` will decipher it.
Answer: `lets try some binary out!`

3. `MJQXGZJTGIQGS4ZAON2XAZLSEBRW63LNN5XCA2LOEBBVIRRHOM======`
This looks some kind of base32. Base32 is a commonly used to transfer files, during transition. `From Base32` in `cyberchef` will convert it back to Plain Text. 
Answer: `base32 is super common in CTF's`

4. `RWFjaCBCYXNlNjQgZGlnaXQgcmVwcmVzZW50cyBleGFjdGx5IDYgYml0cyBvZiBkYXRhLg==`
The two `==` at the end of the cipher denotes it to be a base64. Using `From Base64` in `cyberchef` will convert it back to Plain Text.
Answer: `Each Base64 digit represents exactly 6 bits of data.`

5. `68 65 78 61 64 65 63 69 6d 61 6c 20 6f 72 20 62 61 73 65 31 36 3f`
It has letters and numbers which tells me that it could be hex. Using `From Hex` in `cyberchef` will convert it back to Plain Text.
Answer: `hexadecimal or base16?`

6. `Ebgngr zr 13 cynprf!`
This format seems to be some kind of ROT cipher. The text mentions the number 13, so trying out `ROT13` will decipher the text back to Plain Text.
Answer: `Rotate me 13 places!`

7. `*@F DA:? >6 C:89E C@F?5 323J C:89E C@F?5 Wcf E:>6DX`
I tried it on `ROT Cipher` and worked.
Answer: `You spin me right round baby right round (47 times)`

8. `- . .-.. . -.-. --- -- -- ..- -. .. -.-. .- - .. --- -.. -. -.-. --- -.. .. -. --.`
It looks like a `Morse Code`, so I tried `Morse Code Translator` and got the answer.
Answer: `telecommunication encoding`

9. `85 110 112 97 99 107 32 116 104 105 115 32 66 67 68`
I used `Cipher Identifier` and suggested me that the code could be `Decimal Code.` 
Answer: `Unpack this BCD`

10. LS0tLS0gLi0tLS0gLi0tLS0gLS0tLS0gLS0tLS0gLi0tLS0gLi0tLS0gLS0tLS0KLS0tLS0gLi0tLS0gLi0tLS0g.....
It is a very long string, the `cyberchef` suggests the code to be `Base64.` Once, we decode to `Base64` we get `Morse Code.` After that, we get `Binary Code`. From `Binary`, we get `ROT47` and after decoding that we get `Decimal Code.` Finally, after decoding the `Decimal Code`, we get the answer.
Answer: `Let's make this a bit trickier...`

#### Spectrograms
Download `Audacity` or `sonicvisualiser` to decode the sound.
Answer: `Super Secret Message`

#### Steganography
Open Steganography Decoder and upload the image.
Answer: `SpaghettiSteg`

#### Security through obscurity
Open `StegOnline` and upload the image. Click on the `Show Strings`
Answer: `hackerchat.png`
Answer: `AHH_YOU_FOUND_ME!`

