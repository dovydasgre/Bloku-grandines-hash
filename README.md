## Hash generatorius

# Maišos funkcijos pseudo kodas
<pre>
# Define constants for the initial hash values
HASH_CODE = [
    0x6a09e667, 0xbb67ae85, 0x3c6ef372, 0xa54ff53a,
    0x510e527f, 0x9b05688c, 0x1f83d9ab, 0x5be0cd19
]

# Define a function to mix three uint32_t values
function Mix(a, b, c):
    # Perform bitwise rotations and XOR operations
    temp1 = rotate_right(b, 6) OR rotate_left(b, 32 - 6)
    temp2 = rotate_right(b, 11) OR rotate_left(b, 32 - 11)
    temp3 = rotate_right(b, 25) OR rotate_left(b, 32 - 25)
    s1 = temp3 XOR temp2 XOR b

    temp1 = rotate_right(a, 2) OR rotate_left(a, 32 - 2)
    temp2 = rotate_right(a, 13) OR rotate_left(a, 32 - 13)
    temp3 = rotate_right(a, 22) OR rotate_left(a, 32 - 22)
    s0 = temp3 XOR temp2 XOR temp1

    return a + s0 + b + s1 + c

# Define a function to compute a custom hash for a string
function customHash(input):
    # Initialize an array of 8 uint32_t values with the initial hash values
    hash = copy_of(HASH_CODE)

    # Iterate over each character in the input string
    for each character c in input:
        # Iterate over the 8 uint32_t values in the hash array
        for i from 0 to 7:
            # Update the i-th value in the hash array using the Mix function
            hash[i] = Mix(hash[i], hash[(i + 1) MOD 8], cast_to_uint32(c))

    # Create a string representation of the hash result
    result = ""
    for i from 0 to 7:
        result += format_as_hex(hash[i], 8)

    return result

# Example usage:
hash_result = customHash("your_input_string")
print(hash_result)
</pre>

# Eksperimentinė analizė:
1. Du failai sudaryti tik iš vieno, tačiau skirtingo, simbolio:
- File - oneSymbol1.txt, Hash - cb99ca78e175907b36e841fdcf7dd1db7326eb811c9def5b89f08adc2ce6a645
- File - oneSymbol2.txt, Hash - cb99ca58e175905b36e841ddcf7dd1bb7326eb611c9def3b89f08abc30e6b5e5
2. Du failai sudaryti iš daugiau nei 1000 atsitiktinai sugeneruotų simbolių:
- File - 1001random1.txt, Hash - bae1a0484274b2c886d637565eb05622dcd27f8230dc188f9706d75891f09b15
- File - 1001random2.txt, Hash - 2239de4061446c38e014b2db65f0d4c7c29ba690566929fb1b0ef27430a5098d
3. Du failai sudaryti iš daugiau nei 1000 simbolių, bet skiriasi vienu nuo kito tik vienu simboliu:
- File - oneSymbolDiff1.txt, Hash - 64e115d63d986f8acfb1f4631a0c7fa12f13fca5786d8dc0f4180e4c405ca65e
- File - oneSymbolDiff2.txt, Hash - c664d29d18365c888dc818e1712b53ebcd37159f5c50fc7aaa1dca0bfc37abd9
4. Tuščias failas:
- File - empty.txt, Hash - 6a09e667bb67ae853c6ef372a54ff53a510e527f9b05688c1f83d9ab5be0cd19
5. Efektyvumas:
<br/>![Hash efektyvumas](https://github.com/dovydasgre/blokugrand/assets/126052244/31fb0dd5-20a1-4472-8f24-53a1334621dc)
6. 100 000 atsitiktinių simbolių eilučių porų:
- 25 000 porų, kurių ilgis 10 simbolių, kolizijų skaičius - 0.
- 25 000 porų, kurių ilgis 100 simbolių, kolizijų skaičius - 0.
- 25 000 porų, kurių ilgis 500 simbolių, kolizijų skaičius - 0.
- 25 000 porų, kurių ilgis 1000 simbolių, kolizijų skaičius - 0.
7. 100 000 atsitiktinių simbolių eilučių porų, 32 simbolių eilučių ilgiu, juos skiria tik vienas simbolis. Įvertinamas gautų hash'ų procentinis "skirtingumas" bitų lygmenyje:
- Bitų lygmenyje:
  - Minimali skirtingumo reikšmė: **0%**
  - Maksimali skirtingumo reikšmė: **42.1875%**
  - Vidurinė skirtingumo reikšmė: **32.4734%**
- Hexo lygmenyje:
  - Minimali skirtingumo reikšmė: **0%**
  - Maksimali skirtingumo reikšmė: **100%**
  - Vidurinė skirtingumo reikšmė: **92.23%**

  # Išvados:
  - Algoritmas yra greitas
  - Atstatyti išėjimą į įėjimą neįmanoma
  - Funkcija yra deterministinė
  - Minimalus įėjimo pakeitimai absoliučiai pakeičia išėjimą
  - A ir a simboliai turi panašias hash reikšmes.
 
  # Papildomos užduotys:
1. Mano Hash funkcijos ir SHA256 spartos palyginimas.
  <br/>![comparision](https://github.com/dovydasgre/Bloku-grandines-hash/assets/126052244/345ffa3a-98dc-4afc-af53-ebd0af96722b)

1.1 SHA256 turi pranašumą hash'uojant vieną simbolį.
- File - oneSymbol1.txt, customHash - cb99ca78e175907b36e841fdcf7dd1db7326eb811c9def5b89f08adc2ce6a645
- File - oneSymbol2.txt, customHash - cb99ca58e175905b36e841ddcf7dd1bb7326eb611c9def3b89f08abc30e6b5e5
- File - oneSymbol1.txt, SHA256 - ca978112ca1bbdcafac231b39a23dc4da786eff8147c4e72b9807785afee48bb
- File - oneSymbol2.txt, SHA256 - 559aead08264d5795d3909718cdd05abd49572e84fe55590eef31a88a08fdffd
  SHA256 turi pranašumą hash'uojant vieną simbolį.
1.2 Skirtumo didelio nėra hash'uojant eilutę kurioje skiriasi tik vienas simbolis.
- File - oneSymbolDiff1.txt, customHash - 64e115d63d986f8acfb1f4631a0c7fa12f13fca5786d8dc0f4180e4c405ca65e
- File - oneSymbolDiff2.txt, customHash - c664d29d18365c888dc818e1712b53ebcd37159f5c50fc7aaa1dca0bfc37abd9
- File - oneSymbolDiff1.txt, SHA256 - 46ec492b571fa9226a514a651a00ace1b0494c1acfa4e11a6b536ba47724df84
- File - oneSymbolDiff2.txt, SHA256 - 3019b9c8745801e87a11548b32a4dcbed13a2073571990a254f8a9338d4a82f4
1.3 Skirtumo nėra hash'uojant eilutę, kurioje yra daugiau nei 1000 simbolių.
- File - 1001random1.txt, customHash - bae1a0484274b2c886d637565eb05622dcd27f8230dc188f9706d75891f09b15
- File - 1001random2.txt, customHash - 2239de4061446c38e014b2db65f0d4c7c29ba690566929fb1b0ef27430a5098d
- File - 1001random1.txt, SHA256 - 4b41cf7137e552d024513abb37e972567a409b3232fe24b88afcfe4d3e3c183a
- File - 1001random2.txt, SHA256 - c640948052a7a2837c541cde11f729d3c07bd67021ac2b0cee4b6c31f5224da9

   - 100 000 atsitiktinių simbolių eilučių porų, 32 simbolių eilučių ilgiu, juos skiria tik vienas simbolis. Įvertinamas gautų hash'ų procentinis "skirtingumas" bitų lygmenyje:
## Bitų lygmenyje:
|             | customHash |    SHA256  |  Skirtumas |
|-------------|------------|------------|------------|
|     MIN     |      0%    |     0%     |     0%     |
|     MAX     |   42.187%  |   41.992%  |   0.195%   |         
|     AVE     |   32.473%  |   32.473%  |     0%     |
    
## Hexo lygmenyje:
|             | customHash |    SHA256  |  Skirtumas |
|-------------|------------|------------|------------|
|     MIN     |     0%     |      0%    |     0%     |
|     MAX     |    100%    |     100%   |     0%     |
|     AVE     |   92.23%   |   92.2376% |   0.0076%  |

