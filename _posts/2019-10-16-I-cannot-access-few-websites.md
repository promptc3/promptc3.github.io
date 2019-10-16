---
layout: post
title: "I cannot access few Websites"
tags: [network, ubuntu]
---

# SOLVED: I cannot access some websites on my ubuntu desktop

Recently I updated to Ubuntu 19.04 and encountered a weird problem.
I was able to access sites like reddit and duckduckgo but sites like
netflix and youtube were blocked. At first I though it might be due to
new updates for some reason. Yeah the good ol' blame it on updates.
Then like all intellectuals do I tried to open them on a different
browser. Same problem, that means browsers are safe. Next I tried it
on tor and voila I was able to access them all. So may be my ISP was
the culprit all along. So I tried opening those sites on my android on
same connection. It again worked. Things get interesting here. ISP is
safe. Out of frustration I tried updating again which again led to no
success. Then I tried `ping www.google.com`. Ping worked. More
confusion. Then I tried `wget www.google.com` it didn't work. Even
more stress. By this time I was absolutely clueless and had no idea 
what is happening. The next step was ubuntu forums.
On that sacred place I found this highly sought [question](https://askubuntu.com/questions/957470/cant-access-certain-sites-on-ubuntu-16-04-lts?noredirect=1).
Such a great thread I exclaimed in excitement and bookmarked it so
that I could read later and went to sleep.

After a few days I tried this [solution](https://askubuntu.com/questions/772640/unable-to-access-some-websites).
If you read the solution it says to change the value of **MTU**. I
decreased it as low as 1000 from earlier 1500 and got all those sites back.
Phewww!!!.

## So what is MTU ?
MTU stands for **Maximum Transmission Unit**. From [Wikipedia](https://en.wikipedia.org/wiki/Maximum_transmission_unit)
> It is the size of largest *protocol data unit* that can be communicates in
  a single network layer transaction. It can be adjusted manually for
  routers and other communication interfaces.

## Why lowering the value of MTU worked ?
Turns out like every other possible question in the world of
programming this question had already been answered on a stack forum.
[Link](https://serverfault.com/questions/376752/what-exactly-is-the-mtu-mru-issue-what-it-is-caused-by-and-how-to-fix-it)

It happens because of 'lossy' and 'unreliable' connection. So reducing
MTU means I reduce the size of packets my computer can recieve hence
increasing the 'chances' of receiving the packets. But this increases
overhead hence slower connection.

## Who is the culprit for my frustration then ??
Most likely its **Airtel** my truly unworthy ISP. It was surely
happening due to some broken hop my packets suffered on their way
home, and most likely that was **Airtel**.

## Lessons for future.
* If it seems like a problem with MTU/ICMP etc or it feels like your
  ISP is at fault try `traceroute`. For example `traceroute -F
duckduckgo.com 1472` where `1472` is the value of MTU. Its default
value is `1500 Bytes`. It will tell you about the broken hops.
* Try `ping -c 2 -s 1472 -D google.com`. If it does not succeed try
  lowering the value even further.
* Finally, if you find a suitable value change it using
 `ifconfig eth0 mtu <MTU> up`. Replace `eth0` with your active inerface
name and `<MTU>` with MTU value. For example I used `ifconfig wlp19s0
mtu 1200 up`.

## Summary
Changing MTU is just a workaround and I hope the actual problem is fixed in
future.
