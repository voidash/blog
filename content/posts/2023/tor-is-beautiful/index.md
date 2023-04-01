---
title: "Tor Is Beautiful"
date: 2023-04-01T20:49:32+05:45
draft: false
cover:
  image: "https://i.imgur.com/THkHe2l.png"
---

Looking through the [Google Summer of Code 2023 organizations](https://summerofcode.withgoogle.com/programs/2023/organizations), I came across the Tor Project. The entry to the dark web, the infamous tor where red rooms are hosted, where secret organizations are working on New world order. At least that's the viewpoint of the man who gets his half the knowledge from youtube.

let me clear the distinction between deep web and dark web first. Deep web is indirectly accesible part of the internet. Search engine don't go there. Now why don't search engine look there? Because of `robots.txt` or there might be some form of authentication required to view the content and so on. Now **Dark web** is part of the web that requires **Tor Browser**.

So what is **Tor**? Get this, The project was part of US naval research lab. In 2002 the project was launched as non profit organization dedicated to developing and maintaining the Tor software. So Tor, it's not basically a browser that allows you to view `.onion` links or just another VPN meant for staying anonymous. It is a transparent protocol run on the network built around by tor volunteers. This entire thing is a collaboration at global scale by the people who believe in freedom of speech, and freedom of information.

So when you open up the tor browser and connect to tor service. A tor circuit will be created, which is fancy way of saying your computer will be connected bunch of other computers called relays before touching the internet.

![Connection to internet](https://i.imgur.com/W2EV6Iu.png)
fig: Tor Circuit

Relay is basically a server running tor server software. There are about 6800 relays run on various parts of the world.

- _Guard relay_ needs to be reliable. This is the first entry point to tor network.
- _Middle relay_ : The existence of middle relay makes pinpointing the client very hard.
- _Exit relay_ : These are the relays facing the internet. The FBI raids happen here. You have to respect these people who are running exit relay because they might face frequent raids.

![](https://i.imgur.com/Q8TzlZM.png)
What's behind that onion logo that Tor uses? Once you are connected to tor circuit. You will have the **public keys** for the guard, middle and exit relays. So you encrypt with them

```
encrypt_with_guard_relay_keys(
  encrypt_with_middle_relay_keys(
    encrypt_with_exit_relay_keys(original_data)
    )
  );
```

If you know a thing or two about public-key cryptography then, the things encrypted with public keys can only be decrypted by private keys. So guard relay gets the encrypted data,decrypts it and sends to middle relay. Guard relay doesn't know what the data is as it is still encrypted and can only be decrypted by middle relay. Turn of Middle relay who also decrypts it. Still the data is encrypted..... Time for it to send to exit relay. Exit relay finally decrypts it to send it to internet. Exit relay has the original packet the client sent and it gets forwarded to internet.

It's like peeling the layer of onion.......
![](https://i.imgur.com/EruEK7Y.png)
