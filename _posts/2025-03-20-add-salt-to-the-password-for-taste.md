# Add Salt to the Password for Taste 🧂😉

###### Why Seasoning is Essential in Security

> _“From the earliest times, salt has been a symbol of lasting alliances, sacred rituals, and even rebellion.”_ — [Mark Kurlansky](https://www.markkurlansky.com/), _[Salt: A World History](https://www.markkurlansky.com/books/salt-a-world-history/)_

Salt once shaped empires, sparked wars, and preserved entire civilizations. It helped our ancestors store food during long journeys and harsh winters. In Salt: A World History, Mark Kurlansky traces the remarkable influence of this humble mineral, describing how it carved trade routes, powered economies, and quietly shaped the course of history.

Salt still preserves in our modern, digital world—not food this time, but something just as vital: our online identities. Without salting, even the most complex passwords can be cracked in minutes, leaving personal data exposed and vulnerable.

In this post, we’ll explore how a simple pinch of salt can disrupt even the most sophisticated attacks. If that sounds mysterious or technical, don’t worry — we’ll break it down gently. Think of it like a recipe: no dish is complete without a bit of salt, and no password system is secure without it, either.
<p align="center">
  <img src="https://imgs.xkcd.com/comics/how_hacking_works.png" alt="How Hacking Works" width="512" height="383">
</p>
<p align="center">
  <em> Image Source: <a href="https://xkcd.com/">XKCD</a> </em>
</p>

## The Problem with Passwords

Passwords are one of the oldest tools we have for identification and authentication. If we trace their [history](https://marketing.pinecc.com/blog/the-history-of-passwords), we can go all the way back to the Roman army, where soldiers used _watchwords_ to prove they belonged to a particular [unit](https://www.dashlane.com/a-brief-history-of-passwords). Fast forward a bit, and we get the secret codes whispered at [speakeasies](https://thescotchworld.com/blog/f/the-secret-password-to-enter-the-speakeasy) during Prohibition — the price of admission for a sip of forbidden liquor. The first digital password was [introduced at MIT](https://www.smh.com.au/national/scientist-who-introduced-the-computer-password-20190717-p527zf.html) in 1961 by [Fernando J. Corbató](https://www.nae.edu/29551/Dr-Fernando-J-Corbato), a pioneering computer scientist who wanted to increase access to shared machines while keeping users’ data private.

Passwords have stuck around since then, but they’ve also become one of the weakest links in our digital defenses. On a human level, most of us reuse the same password across multiple accounts or choose ones that are far too easy to guess (_password123_, _iloveyou_, etc.). Even when we try to be clever, the truth is — humans aren’t great at randomness. And hackers know that.

Worse still, some systems continue to store passwords in plain text. Yes, in plain text — as recently as 2019, [Meta (the parent company of Facebook)](https://krebsonsecurity.com/2019/03/facebook-stored-hundreds-of-millions-of-user-passwords-in-plain-text-for-years/) discovered it had stored over 600 million user passwords in searchable plain text files dating back years. In 2021, a [GoDaddy breach](https://www.wordfence.com/blog/2021/11/godaddy-breach-plaintext-passwords/) exposed the plain-text passwords of 1.2 million users. Storing passwords in plain text is exactly as bad as it sounds. It’s like writing down your deepest secrets in a diary and leaving it unlocked on a public bench. If anyone gains access, they can read everything. To fix that, developers use [hashing](https://supertokens.com/blog/password-hashing-salting), which turns passwords into scrambled, fixed-length codes that are difficult to reverse. For example, instead of storing this:

| User Name | Password                    |
| --------- | --------------------------- |
| User1     | ItIsWhatItIs                |
| User2     | MyVeryStrongPassword345!@23 |
| User3     | Password1                   |
| User4     | ILoveThisWebsite            |

We use a hashing function to transform it into this:

| User Name | Hashed Password                                              |
| --------- | ------------------------------------------------------------ |
| User1     | $2y$10$xKySiD8229yRa.zimaCd.eEEOKi057YSMJqH.doAiXnIAI5pIPySK |
| User2     | $2y$10$jbbc6lg0Yo3wdA8WKVy7B.AYd5Y4udMs6XcPb2uvhCM2bd7oSTtm. |
| User3     | $2y$10$CZRnyru6S3pEFsTRbIPZ.eFSlntphgVURyy8qrtNXSSovAi4JfIb6 |
| User4     | $2y$10$UBVekw9siGufgeM.3nbye.WspumDpK0LVgW7PJAGZBJ0qtXyqZKdu |

This way, even if an attacker gets the data, they see only hashes — not the actual passwords. When a user logs in, the password they enter is hashed, and the system compares it to the stored hash.

But hashing isn’t bulletproof. If two users choose the same password, their hashes will be identical — making it easier for attackers to spot patterns. Hackers also use [rainbow tables](https://www.strongdm.com/what-is/rainbow-table-attack) , which are massive precomputed lists of common passwords and their corresponding hashes. And brute-force attacks like [dictionary attacks](https://www.kaspersky.com/resource-center/definitions/what-is-a-dictionary-attack) can try billions of combinations per second. The more predictable the system — or the hashing function — the easier it is for attackers to win.

So how do we throw them off the scent? How do we make even predictable passwords harder to crack?

That’s where **salt** comes in.
<p align="center">
  <img src="https://res.cloudinary.com/dm60dcmf7/image/upload/v1743342362/SCR-20250330-gnfn_kwmfme.jpg" width="410" height="407">
</p>

## So... What is a Salt?
In the world of password security, a **salt** is a random string of characters added to a password _before_ it’s hashed. It doesn’t have to be long or fancy — though a long enough salt almost eliminates the risk of a rainbow table attack. This tiny addition completely changes the outcome of the hash, even if two users pick the exact same password.

Think of it like a secret spice in a recipe. Two chefs might start with the same dish, but if one adds cinnamon and the other adds chili, you’ll end up with very different flavors. That’s what salt does: it makes every “dish” — in this case, every password hash — unique.

Let’s break it down with an example.

Suppose two users both choose the same (very strong!) password: `password`.

If we hash it without any salt, they’ll get the exact same result:

```text
Hash("password") → $2y$10$LYkedKLZX3qDb06Hd17zWuybj9EhqUWKFjOjqSRBKF6q3rE3ldcsq
```

Now, let’s say we add a unique salt for each user:

```text
User1: Salt = X8g72!
Hash("X8g72!password") → $2y$10$DebUSiinkd2GvJZ0uqgsFOvvfDClW.kvnZ7R3g/DLmMh/XySy00hu
```

```text
User2: Salt = Lz91#k
Hash("Lz91#kpassword") → $2y$10$mIzd3Z5y.dwyTVD.EpCeV.vZOp8dPUSzY3SRt.Whrj.o8kDAIDhaq
```

Even though both users chose the same password, their final hashes are completely different. That means a hacker can’t easily tell they started from the same input. And because salts are generated randomly, rainbow tables become practically useless — an attacker would have to compute a new table for every possible salt, which is computationally impractical.

Using the earlier example from the hashing section, we can now see how salting changes what’s stored in the database. First, here’s how we build the salted password:

| User Name | Password                    | Salt   | Salted Password                   |
| --------- | --------------------------- | ------ | --------------------------------- |
| User1     | ItIsWhatItIs                | aQ4$eP | ItIsWhatItIsaQ4$eP                |
| User2     | MyVeryStrongPassword345!@23 | rT9^xB | MyVeryStrongPassword345!@23rT9^xB |
| User3     | Password1                   | D3@u1Z | Password1D3@u1Z                   |
| User4     | ILoveThisWebsite            | Mn*7cV | ILoveThisWebsiteMn*7cV            |

And here’s how we safely store both the salt and the resulting hash:

| User Name | Salt   | Hashed Password                                              |
| --------- | ------ | ------------------------------------------------------------ |
| User1     | aQ4$eP | $2y$10$4dgl.RSifOPIrB29WzKOB.i60f2Z8up6jwRp6dSdpPe9p/ryTU8je |
| User2     | rT9^xB | $2y$10$s07QB8CrMackdGawjJpFIO3rPWPrczd31LSQR20Pdz8cYtEXGXG7W |
| User3     | D3@u1Z | $2y$10$IeXB26bNQjj96CPML5s50OM6RyZxhT2/p7mY.V7gaT8kEe34GTU7O |
| User4     | Mn*7cV | $2y$10$EE2Ga1X.qaUsnyzZCvO79.raQwn1IcThHq3VMk52AB0/a2NJedeFm |

By storing the salt alongside the hash, systems can reliably re-hash a password during login for comparison without compromising security. The salt itself doesn’t need to be secret. It just needs to be unique and unpredictable.

It’s important to emphasize, however, that [salting](https://auth0.com/blog/adding-salt-to-hashing-a-better-way-to-store-passwords/) doesn’t make passwords stronger. It only makes attacks harder — and that distinction matters.

## Common Mistakes

<p align="center">
  <img src="https://imgs.xkcd.com/comics/password_strength.png" alt="Password Strength" width="512" height="383">
</p>
<p align="center">
  <em> Image Source: <a href="https://xkcd.com/">XKCD</a> </em>
</p>

> "You are the salt of the earth. But if the salt loses its saltiness, how can it be made salty again? It is no longer good for anything, except to be thrown out and trampled underfoot." - Jesus, *Matthew 5:13*

Like any good recipe, salting only works if you do it right. [One of the biggest mistakes](https://stackoverflow.com/questions/24415950/is-it-an-good-idea-of-using-password-itself-as-an-salt) is reusing the same salt for every user — which completely defeats the purpose. If everyone gets the same seasoning, all the hashes for identical passwords will still look the same. That’s no better than unsalted hashing. Salts must be unique per user — and ideally unique per password reset — to offer real protection.

Another common pitfall is using salts that are **too short**, **predictable**, or **not truly random** (we can talk about generating secure random values in a follow-up post). A salt like `123456` or `admin` isn’t fooling anyone — certainly not a hacker with a rainbow table and some spare time. Ideally, a good salt should come from a strong random number generator, be at least 16 characters long, and draw from a broad character set. You can dig deeper in [this Stack Overflow conversation](https://stackoverflow.com/questions/184112/what-is-the-optimal-length-for-user-password-salt) on optimal salt lengths.

Other frequent missteps include forgetting to store the salt or storing it insecurely. While the salt doesn’t need to be hidden, it _does_ need to be reliably retrievable. If you lose or corrupt it, you cannot re-hash and verify a user’s password correctly—it’s like losing the recipe for your signature dish.

And finally, one of the most unfortunate (and surprisingly common) mistakes is hashing without salting. (You can find examples in the reference links below.) This still shows up in legacy systems or rushed codebases — and it leaves users dangerously exposed to the kinds of attacks we’ve already discussed. Modern systems should always salt and hash passwords with a strong algorithm like [Argon2](https://www.argon2.com/), or [bcrypt](https://nordvpn.com/blog/what-is-bcrypt). Even though limitations in bcrypt contributed to the recent [Okta security vulnerability](https://trust.okta.com/security-advisories/okta-ad-ldap-delegated-authentication-username/), it’s still widely used — and I have some thoughts on Okta Security coming soon, so stay tuned). 

For practical implementation tips, I also highly recommend the [OWASP Password Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html). 

## Bonus: Salt Alone Isn't Enough (Use Pepper too)

Salt is powerful — but it’s not a magic shield. While it protects against attacks like rainbow tables and ensures every password hash is unique, it’s just one part of a broader security recipe.

Some systems add another secret ingredient: a **pepper**. Like salt, a pepper is a string added to the password before hashing — but unlike salt, the pepper is kept secret and shared across all users. Think of it as a house spice blend only the kitchen staff knows. Even if attackers access the database, they still need the pepper to recreate the hashes.

That said, pepper comes with tradeoffs. Because it must be stored securely (usually in the application code or an environment variable), managing it introduces additional complexity. As with any ingredient, there’s ongoing debate—[some security experts](https://blog.ircmaxell.com/2012/04/properly-salting-passwords-case-against.html) argue that pepper doesn’t always add meaningful protection and may not be worth the added risk.

Then there’s the matter of _how_ we hash. Not all algorithms are created equal. General-purpose functions like SHA-256 are fast — which is excellent for performance, but unfortunately, it’s also great for attackers. That’s why we use key-stretching algorithms like [bcrypt](https://nordvpn.com/blog/what-is-bcrypt), [scrypt](https://en.wikipedia.org/wiki/Scrypt), or [Argon2](https://www.argon2.com/), which are intentionally slow. These algorithms increase the cost of each password guess, making brute-force and dictionary attacks much less practical.

And, of course, password storage is just one layer of defense. A secure system should also include rate limiting (to prevent automated guessing or bot-like behavior), multi-factor authentication (MFA) (so a stolen password alone isn’t enough), and regular audits of how passwords and credentials are managed.

Security is layered. Salt is essential, but it can’t carry the weight alone. It works best when combined with solid algorithms, thoughtful design, and strong overall hygiene. Like seasoning a meal, security is not just about one ingredient—balance, technique, and attention to detail.

## Conclusion: Security Recipe Card

>  As a bonus, I created the hashes in the examples above using one of the common key-stretching algorithms. If you can figure out which one, let me know. I’d be curious to learn how you did it.

Salting might seem like a quirky detail in the vast world of cybersecurity, but as we’ve seen, it’s one of the simplest and most powerful tools we have for protecting passwords. By adding randomness to each hash, salts break patterns, frustrate attackers, and help ensure that even identical passwords don’t leave identical traces.

For developers, salting should be a non-negotiable part of your password storage process. And for the curious reader — whether you’re learning, building, or just staying security-conscious — it’s worth digging into how your favorite platforms handle password protection. Do they use proper salting? Do they mention bcrypt or Argon2? Is MFA available? These are questions worth asking.

Password security doesn’t have to be complicated — but it _does_ have to be thoughtful. So yes: salt your food, and your passwords.

Finally, while this post focused on password security, my next post will take a deeper look at digital identity — and how it shapes, protects, and sometimes complicates our lives online. Hope to see you there.

Till then, stay salty, stay safe. 🧂😌

## References and Resources to Learn More

- [Hackers Stole Access Tokens from Okta’s Support Unit](https://krebsonsecurity.com/2023/10/hackers-stole-access-tokens-from-oktas-support-unit/)
- [Unpacking the Okta Data Breach](https://www.portnox.com/blog/cyber-attacks/unpacking-the-okta-data-breach/)
- [Is salted MD5 or salted SHA considered secure?](https://security.stackexchange.com/questions/61489/is-salted-md5-or-salted-sha-considered-secure)
- [Why an unsalted MD5 hash is bad practice](https://svanas.medium.com/why-an-unsalted-md5-hash-is-bad-practice-6a0d7d017856)
- [How NOT to Store Passwords! - Computerphile](https://youtu.be/8ZtInClXe1Q?feature=shared)
- [ELI5: What does it mean if a password is stored as unsalted MD5 hash?](https://www.reddit.com/r/explainlikeimfive/comments/tgn650/eli5_what_does_it_mean_if_a_password_is_stored_as/)
- [What does it mean if a password is stored as an unsalted MD5 hash?](https://www.quora.com/What-does-it-mean-if-a-password-is-stored-as-an-unsalted-MD5-hash)
- [The Evolution of Password Hashing](https://psono.com/blog/evolution-of-password-hashing)
- [Legacy app with md5 hashes: how to add salt and SHA1?](https://stackoverflow.com/questions/10992262/legacy-app-with-md5-hashes-how-to-add-salt-and-sha1)
- [What is bcrypt and how does it work?](https://nordvpn.com/blog/what-is-bcrypt)
- [Okta AD/LDAP Delegated Authentication - Username Above 52 Characters Security Advisory - Nov 1, 2024](https://trust.okta.com/security-advisories/okta-ad-ldap-delegated-authentication-username/)
- [How Bcrypt’s Limitations Contributed to Okta’s Vulnerability: A Lesson for Developers](https://medium.com/@rajat29gupta/how-bcrypts-limitations-contributed-to-okta-s-vulnerability-a-lesson-for-developers-39425c644ed5)
- [Is it an good idea of Using password itself as an salt](https://stackoverflow.com/questions/24415950/is-it-an-good-idea-of-using-password-itself-as-an-salt)
- [Best Practices: Salting & peppering passwords?](https://stackoverflow.com/questions/16891729/best-practices-salting-peppering-passwords)
- [Password Hashing and Storage Basics](https://markilott.medium.com/password-storage-basics-2aa9e1586f98)
- [Password Salting](https://www.techtarget.com/searchsecurity/definition/salt)
- [Interesting Conversation from Y Combinator News](https://news.ycombinator.com/item?id=2005017)
- [Adding Salt to Hashing: A Better Way to Store Passwords](https://auth0.com/blog/adding-salt-to-hashing-a-better-way-to-store-passwords/)
- [What is a rainbow table attack and how to prevent it?](https://proton.me/blog/what-is-rainbow-table-attack)
- [Salt – Preventing Rainbow Attacks against Password Stores](https://sqlity.net/en/2309/salt/)
- [Salt Hashing and How It Can Lower The Chances of Exposing Login Credentials](https://optimalidm.com/resources/blog/what-is-salt-hashing/)
- [The History and Future of Passwords](https://www.beyondidentity.com/resource/the-history-and-future-of-passwords)
- [The World's First Computer Password? It Was Useless Too](https://www.wired.com/2012/01/computer-password/)
- [(Part I of III) The History of Passwords](https://marketing.pinecc.com/blog/the-history-of-passwords)
- [The Secret Password to Enter the Speakeasy](https://thescotchworld.com/blog/f/the-secret-password-to-enter-the-speakeasy)
- [Meta stored 600 million Facebook and Instagram passwords in plain text](https://appleinsider.com/articles/24/09/27/meta-stored-600-million-facebook-and-instagram-passwords-in-plain-text)
- [A Brief History of Passwords](https://www.dashlane.com/a-brief-history-of-passwords)
- [Facebook Stored Hundreds of Millions of User Passwords in Plain Text for Years](https://krebsonsecurity.com/2019/03/facebook-stored-hundreds-of-millions-of-user-passwords-in-plain-text-for-years/)
