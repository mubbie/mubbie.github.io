# URL (or Percent) Encoding

###### Why URLs Speak in Code

<p align="center">
  <img src="https://imgs.xkcd.com/comics/new_bug.png" alt="Pooh Recursion" width="657" height="268">
</p>

<p align="center">
  <em> Image Source: <a href="https://xkcd.com/1700/">XKCD</a>
  </em>
</p>

# Introduction

Have you ever clicked on a link or typed a web address only to see something strange in the URL? Maybe it looked like this:

```
https://internet.com/explore/search?query=what+is+a+good+application+of+N%C3%A4ive+Bayes+in+deep+learning
```

when your search query was: `what is a good application of Näive Bayes in deep learning`. Or perhaps you encountered something like this:

```
https://internet.com/travel/hotels/courtyard%20by%20marriot%20D%C3%BCsseldorf
```

when you were looking for Marriot hotels in [Düsseldorf, Germany](https://www.visitduesseldorf.de/en).

What are all those `%` signs and numbers? Are they some kind of secret code? In this blog post, we’re going to unravel the mystery behind these symbols and take a deep dive into URL encoding, ASCII, and the fascinating story of URLs themselves.

## What are URLs?

A [URL (Uniform Resource Locator)](https://youtu.be/nLYlurfEDSI?si=rfE66BMSYoGhrxa7) is the cornerstone of how we navigate the web. Simply put, a URL is an address that tells your browser where to find a specific resource, such as a webpage, an image, or a file. You can think of it as a digital version of a friend’s home address—it ensures that when you need something from your friend, you know precisely where to go to receive the article reliably.

A typical URL would look something like this: 

```
https://www.internet.com:8080/path/to/resource?query=parameter#fragment
```

Here's the breakdown:

<p align="center">
  <img src="https://res.cloudinary.com/dm60dcmf7/image/upload/v1735335003/URI_syntax_diagram_k0pcsq.svg" width="800" height="75">
</p>

<p align="center">
  <em> Image Source: <a href="https://en.wikipedia.org/wiki/File:URI_syntax_diagram.svg">Wikipedia</a>
  </em>
</p>


- **Protocol/URI Scheme**(`https://`): Specifies how your browser should communicate with the internet server. Common protocols include `http`/`https` (secure `http`), and `ftp` (file transfer), `mailto` (email address) and more. While network protocols are not the focus of this blog post, you can learn more about them in these articles: [Types of Network Protocols and Their Uses](https://www.geeksforgeeks.org/types-of-network-protocols-and-their-uses/), [What is a network protocol?](https://www.cloudflare.com/learning/network-layer/what-is-a-protocol/), [Uniform Resource Identifier (URI) Schemes](https://www.iana.org/assignments/uri-schemes/uri-schemes.xhtml)
- **Domain**(`www.internet.com`): Identifies the server hosting the internet resource. This is typically a human readable name that maps to an IP address via DNS (Domain Name System). For a given domain, you can easily look up the corresponding IP address in DNS. For instance, on my local machine, I can use the `host` command or the `nslookup` utility to take a look at the IP addresses for Google servers. Public tools like [DNS Checker](https://dnschecker.org/) and [MX Toolbox](https://mxtoolbox.com/SuperTool.aspx) are also handy for peeking at DNS records for domains.
- **Port**(`:8080`): Optional and specifies which port the server should use. The default for `http` is 80, and for `https` it's 443. Other port numbers and the correspodning protocals are `21` for `ftp`, and `22` for `ssh`, etc. The port essentially specifies which application or service on a server to connect to via the URL. If the URL is your friend’s home address, the port number can be thought of as your friend’s room.
- **Path**(`/path/to/resource`): Indicates the specific location of the resource on the server. It's like the folders and files on your computer.
- [**Query**(`?query=parameter`)](https://www.branch.io/glossary/query-parameters/): A set of key-value pairs used to pass information to the server. For example, a search query when we want to learn about Näive Bayes or a users preferences when making a request from the server.
- [**Fragment**(`#fragment`)](https://medium.com/@dhanukasn/understanding-query-parameters-and-uri-fragments-in-urls-f6c52034b634): Refers to a specific section within the resource. For instance, it could be specific lines in a text file or a particular section or bookmark on a webpage.

## Where Did URLs Come From?

<p align="center">
  <img src="https://res.cloudinary.com/dm60dcmf7/image/upload/v1735339177/Tim_Berners-Lee_hf08cs.jpg" alt="The World Wide Web was invented by British scientist Tim Berners-Lee in 1989 while working at CERN" width="403" height="259">
</p>

<p align="center">
  <em> Image Source: <a href="https://www.home.cern/science/computing/birth-web">CERN</a>
  </em>
</p>

The concept of URLs was introduced alongside the HTTP protocol and HTML in 1992 by [Tim Berners-Lee](https://www.w3.org/People/Berners-Lee/), way for researchers to easily share and access documents.

Berners-Lee has already proposed the ideas of the [World Wide Web](https://www.w3.org/History/1989/proposal.html). However, for this network to function, it needed a standardized way to identify and locate resources. To address this need, he proposed the idea of the [URL](http://1997.webhistory.org/www.lists/www-talk.1991/0018.html) to serve as a "document identifier." The URL became one of the three core components of the web, alongside **HTML** (for structuring documents) and **HTTP** (for transferring them).

Initially, URLs were simple and primarily used to point to static files hosted on servers [Over time](https://blog.cloudflare.com/the-history-of-the-url/), they evolved alongside the web into a versatile system supporting dynamic content, user inputs, and even encrypted communication. Throughout this evolution, the central principle of universality—the idea that URLs should work on any device and in any context—has stood the test of time.

## ASCII and Its Role in URL Encoding

To understand the “strange” encodings we see in URLs, we need to look back at the history of [ASCII](https://www.britannica.com/topic/ASCII). The American Standard Code for Information Interchange (ASCII) was developed in the [1960s](https://www.ascii-code.com/timeline) as a standardized way for computers to represent text characters. Before ASCII, there were numerous incompatible encoding systems, which made it challenging for different systems to communicate. ASCII changed that by providing a universal 7-bit character set, which could represent 128 unique characters.

This [128-character set](https://www.ascii-code.com/) included:
- **Printable characters:** Uppercase (`A-Z`), lowercase (`a-z`), digits (`0-9`), and symbols like `@`, `#`, and `$`, etc.
- **Control characters:** Instructions for managing text streams, such as newline (`\n`) and tab (`\t`).

ASCII’s simplicity and universality made it the foundation for early computer systems and networks, including the internet. However, its biggest limitation was its inability to represent non-English characters, like `é`, `ß`, or `中`, as well as other writing systems like Cyrillic, Arabic, and Chinese. The biggest reason for this limitation is because at the time it was invented, memory and processing power were incredibly expensive; hence every it mattered. By sticking to 7-bits, and thus, 128 characters, [ASCII struck a balance between functionality and efficiency](https://randomtechnicalstuff.blogspot.com/2009/05/unicode-and-oracle.html). It was small enough to fit into the limited storage and memory of the time, yet comprehensive enough to provide a range of characters to work with. Likewise, it was a light-weight, simple, easy-to-implement solution and universal (at least for English-speaking developers).

### Expanded Character Sets (Beyond ASCII)

<p align="center">
  <img src="https://imgs.xkcd.com/comics/standards.png" alt="Standards" width="500" height="283">
</p>

<p align="center">
  <em> Image Source: <a href="https://xkcd.com/1700/">XKCD</a>
  </em>
</p>

As the internet connected the world, the need for a broader character set became apparent. This led to the development of [Unicode](https://www.translationroyale.com/the-history-of-unicode/), which could represent virtually every character in [every language](https://youtu.be/MijmeoH9LT4?feature=shared). [Unicode](https://home.unicode.org/) works with [multiple encodings](https://youtu.be/GMF2Z1EZHXk?si=5Q2JBozHR_LY3UAJ), such as UTF-8, UTF-16, and UTF-32 (You can read more about them here: [Difference between UTF-8, UTF-16 and UTF-32 Character Encoding? Example](https://javarevisited.blogspot.com/2015/02/difference-between-utf-8-utf-16-and-utf.html))

**UTF-8** is the most widely used encoding on the web today. It is backward-compatible with ASCII, meaning that all ASCII characters retain their original binary values, while non-ASCII characters are represented using additional bytes. For example:

- The ASCII character `A` remains `01000001` in binary under UTF-8. 
- The Unicode character `é` is represented as `11000011 10101001` in UTF-8.

Despite Unicode’s dominance in modern text representation, URLs remain constrained to ASCII due to backward compatibility and simplicity. Early internet protocols, including URLs, were built on ASCII. Changing this foundation would disrupt countless systems and applications.

To address this, the solution was **percent-encoding**, or **URL encoding**, which allows characters outside the ASCII range to be safely represented in URLs.

## URL Encoding

URL encoding ensures that any character—whether it's [unsafe](https://www.ietf.org/rfc/rfc1738.txt#:~:text=Other%20characters%20are%20unsafe%20because,be%20encoded%20within%20a%20URL.), [reserved](https://www.ibm.com/docs/en/cics-ts/6.x?topic=concepts-reserved-excluded-characters), or [non-ASCII](https://rbutterworth.nfshost.com/Tables/compose/)—can be safely transmitted in a URL. Here's how it works:

1. **Identity Characters to Encode:**
    - **Reserved Characters:** Characters with special meanings in URLs (e.g., `?` to start a query string, `&` to seperate parameters, `/` to seperate path components, etc.) must be encoded when used outside their context.
    - **Unsafe Characters:** Characters like `spaces`, `<`, `>`, `{`, `}`, etc. are unsafe because gateways and transport agents might modify them. Encoding prevents such misinterpretation.
    - **Non-ASCII Characters:** These characters, which fall outside the ASCII set, must be encoded for compatibility across systems.

2. **Convert Characters to Hexadecimal ASCII:**
    - Each character is replaced with a `%` followed by its two-digit hexadecimal value. (Use this handy [URL Encoding Reference](https://www.w3schools.com/tags/ref_urlencode.ASP)) mapping characters to their hexadecimal values.

### Details Examples of URL Encoding

**Reserved/Unsafe Characters in Query Strings**

```
Original: https://internet.com/search?q=C++&C Programming
Encoded:  https://internet.com/search?q=C%2B%2B%26C%20Programming
```

Here,
- `+` becomes `%2B` (reserved character).
- `&` becomes `%26` (reserved character).
- `Space` (` `) becomes `%20` (unsafe character).

**Non-ASCII Characters**

```
Original: https://internet.com/profile?name=Günter Hernández François
Encoded:  https://internt.com/profile?name=G%C3%BCnter%20Hern%C3%A1ndez%20Fran%C3%A7ois
```

In this case,
- `ü` is encoded as `%C3%BC`.
- `á` is encoded as `%C3%A1`.
- `ç` is encoded as `%C3%A7`.
- `Space` is encoded as `%20`.

Below are examples of encodings for some reserved characters:

| Character | ASCII Value | Encoded Value |
|-----------|-------------|---------------|
| `,`       | `44` (0x2C) | `%2C`         |
| `/`       | `47` (0x2F) | `%2F`         |
| `:`       | `58` (0x3A) | `%3A`         |
| `;`       | `59` (0x3B) | `%3B`         |
| `=`       | `61` (0x3D) | `%3D`         |
| `?`       | `63` (0x3F) | `%3F`         |
| `@`       | `64` (0x40) | `%40`         |
| `[`       | `91` (0x5B) | `%5B`         |
| `]`       | `93` (0x5D) | `%5D`         |

And correspondingly, for some non-ASCII Unicode characters:

| Character | Description               | Unicode Code Point | UTF-8 Encoding | URL Encoded Value |
|-----------|---------------------------|---------------------|----------------|-------------------|
| `ç`       | Latin small c with cedilla| `U+00E7`            | `C3 A7`        | `%C3%A7`          |
| `é`       | Latin small e with acute  | `U+00E9`            | `C3 A9`        | `%C3%A9`          |
| `ß`       | German Eszett (sharp S)   | `U+00DF`            | `C3 9F`        | `%C3%9F`          |
| `中`       | Chinese character         | `U+4E2D`            | `E4 B8 AD`     | `%E4%B8%AD`       |
| `π`       | Greek small letter pi     | `U+03C0`            | `CF 80`        | `%CF%80`          |
| `🎉`      | Party popper emoji         | `U+1F389`           | `F0 9F 8E 89`  | `%F0%9F%8E%89`    |
| `🚀`      | Rocket emoji               | `U+1F680`           | `F0 9F 9A 80`  | `%F0%9F%9A%80`    |

# Conclusion

URL encoding is a critical mechanism that allows URLs to safely transmit a wide range of data while maintaining compatibility with the internet’s ASCII-based foundation. So, the next time you see `%20` or `%C3%A9` in a URL, don't panic—it’s just the internet’s way of speaking a language that devices all around the globe can understand.
