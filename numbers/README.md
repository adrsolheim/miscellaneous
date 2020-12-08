## Preface

* To the computer there is no meaningful conversion between binary, hex, octal, etc. The computer only operates on binary. If there is a conversion somewhere then it is **we** that want another representation of some (binary) number, e.g. we want to express a big number as a more readable hexadecimal string. 
* The hexadecimal represents a big number, and the binary form of this hexadecimal number represents the same big number.  
* Don't confuse hexadecimal characters with ASCII characters. 
	* `0xA == 10`
	* `char('A') == 65`, only used to print when representing the hexadecimal value

### Bitwise <<

Shifts its first operand left by a number of bits given by its second operand, filling in 0s at the right
```
0101 0110 << 2 // 0001 0101 1000
```

### Bitwise >>
Shifts its first operand right by a number of bits given by its second operang, filling in **the same bit as the signing bit** at the left. 
```
0101 0110 >> 1 // 0010 1011
1001 1100 >> 1 // 1100 1110
```

### Bitwise >>>
Shifts does not use the sign bit to fill the trailing positions (always uses 0)
```
0001 0010 >>> 1 // 0000 1010
1110 0010 >>> 1 // 0111 0001
```

### Hex

Hexadecimal is similar to the octal numeral system (base 8). Hexadecimal uses four-bit binary encoding (each hex digit is equal to four digits in binary)

### Octal 

Octal uses a three-bit binary system (each octal digit is equal to three digits in binary)

### Binary to Hex
A hex digit can be any character from `A-F` and `0-9`.
In `ASCII` code that results in `65-70` and `48-57` respectively.  
This means we will be able to fit 2 hexadecimal digit within one byte (70 + 70 < 255).

```
    import java.nio.charset.StandardCharsets;

    // !! byte array of the numeric hexadecimal values, not the ASCII values
    public static String binaryToHex(byte[] bytes) {
        // Index [0-15] == [0x00-0x0F]
        // Contains ASCII code for each character
        byte[] HEX_ARRAY = "0123456789ABCDEF".getBytes(StandardCharsets.US_ASCII);
        // 2 hex digits = 1 byte
        char[] hexCharacters = new char[bytes.length * 2];
        for (int i = 0; i < bytes.length; i++) {
            int v = bytes[i] & 0xFF;
            // 1111 1101 -> 0000 1111
            hexCharacters[i*2] = (char) HEX_ARRAY[v >>> 4];
            // 1111 1101 -> 0000 1101
            hexCharacters[i*2+1] =  (char) HEX_ARRAY[v & 0x0F];
        }
        return new String(hexCharacters);
    }
```

### Hex to Binary

```
    // "ABAB1212"
    public static byte[] hexToBinary(String hexstr) {
        byte[] HEX_ARRAY = "0123456789ABCDEF".getBytes(StandardCharsets.US_ASCII);
        char[] hex = hexstr.toUpperCase().toCharArray();
        if (hex.length % 2 != 0) {
            throw new IllegalArgumentException("Hexadecimal string argument cannot be of odd length. ");
        }
        byte[]  bytes = new byte[hex.length / 2];
        for (int i = 0; i < hex.length  / 2; i++) {
            int b;
            if (hex[2*i] > 64) {
                b = (int) hex[2*i] - 55;
                b = b << 4;
            } else {
                b = (int) hex[2*i] - 38;
                b = b << 4;
            }
            if (hex[2*i+1] > 64) {
                b = b | (hex[2*i+1] - 55);
            } else {
                b = b | (hex[2*i+1] - 38);
            }
            bytes[i] = (byte) b;
        }
        return bytes;
    }
```
