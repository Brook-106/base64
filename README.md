# Base64

A simple Base64 encoder and decoder that runs in the browser. I wrote the encoding and decoding myself following RFC 4648, so it doesn't use `btoa`, `atob` or any library.

Live version: https://Brook-106.github.io/base64/

## What it can do

- Encode and decode text (UTF-8 is handled properly)
- Encode any file, or load a Base64 file to decode it, and save the result
- Standard and URL-safe alphabets, with padding on or off
- Decode input that uses either alphabet, has no padding, or contains line breaks
- Tell you which character is wrong when the input isn't valid Base64
- Show binary output as hex, and download the real bytes with Save
- Step through each 3-byte chunk to see how the bits get regrouped
- Follow your system's light or dark theme

Everything happens in your browser. Nothing gets uploaded.

## How Base64 works

Base64 takes 3 bytes (24 bits) at a time and splits them into four 6-bit groups. Each group is a number from 0 to 63, which picks a character from a 64-character alphabet.

```
Input      H         i         !
Bytes      01001000  01101001  00100001
Regroup    010010  000110  100100  100001
Index      18      6       36      33
Output     S       G       k       h        ->  SGkh
```

If the last chunk has fewer than 3 bytes, zeros are added to fill it and the output is padded with `=`.

| Bytes left | Characters | Padding |
|-----------:|-----------:|:-------:|
| 3          | 4          | none    |
| 2          | 3          | `=`     |
| 1          | 2          | `==`    |

Decoding does the same thing in reverse. Each character becomes 6 bits, four of them make 24 bits, and those split back into 3 bytes. Any bytes that only came from padding are thrown away.

## Running it locally

There is nothing to build. Open `index.html` in a browser, or serve the folder:

```
python3 -m http.server 8000
```

Then go to http://localhost:8000.

## Hosting on GitHub Pages

1. Put `index.html` and this README in the root of the `base64` repo.
2. Go to Settings, then Pages.
3. Under Build and deployment, pick "Deploy from a branch", choose `main` and `/ (root)`, and save.
4. Wait a minute or two and it will be live at `https://<username>.github.io/base64/`.

## Files

- `index.html` has the whole app (HTML, CSS and JavaScript in one file)
- `README.md` is this file

## Notes

- The fonts (IBM Plex) come from Google Fonts. If you don't want any external requests, delete the font `<link>` tags in the `<head>` and it will use system fonts.
- Base64 is an encoding, not encryption. Anyone can decode it, so don't use it to hide anything.
