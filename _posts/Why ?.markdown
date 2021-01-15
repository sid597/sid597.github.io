---
layout: post
category: distributed systems
title: Why ?
data: 2020-12-30
---

I made a website which does text-to-speech for any wikipedia page with natural sounding voice plus some additional features. The way I modeled my data allows me to add a bunch of servers if traffic is high, there are also no consistency issues with adding more servers. So after I made that website most of the websites were CRUD apps to me, I thought I can make a social network with basic features (authentication, posting, commmenting, followers/friends) and this idea seemed workable, but only on <span style="color:green">ONE SERVER</span>. 


Let's say the social network website consists of : Web servers as controller and database servers as Model in MVC framework. Now let's assume at some point website traffic drastically grows then web servers will become bottleneck because even the most powerful server would be able to serve only some X no of requests and if the number of requests are more than that then we would need additional web server, similarly at some point the database server would become bottleneck by serving to  web servers. 
Now what we want to do in case of web or database server becomes a bottleneck is buy more servers and divert some traffic to these new servers. 

But this is where a few problems start arising: 
In case of 1 server we know that if someone made a post/comment everyone else can see those changes i.e every read sees the most recent writes. Now let's say we have 2 db servers A, B and some user X is making changes to A because this server is closer to them and for performance we let them connect to this server.
For some other user Y server B is closer. So a few questions we want to ask are :
1. How are X's changes reflected to Y and vice versa?
2. What happens if A crashes or there is network partition between A and B ?
3. Does Y always see fresh data after Y makes some changes ?
4. Like in my text-to-speech website does adding more servers get better performance ?

These were the questions I started out with one can find other more better reasons for why distributed systems. I also wanted to understand what makes implementing big websites hard and what's down there behind the cloud abstraction.

<b>From [MIT 6.824 Course][6.824]</b>

> If you are designing a system to solve a problem, and you can possibly solve it on a single computer, do it that way it always easier. Try everything else before you try building out distributed systems
 - [Prof. Robert Morris][Prof. Robert]

The reason why people are driven to use lots of cooperating computers are:

1. They need to get high-performance and the way to think about that is they want to  achieve some sort of parallelism lots of CPUs lots of memories lots of disk arms moving in parallel. 
2. To tolerate faults have two computers do the exact same thing if one of them fails you can cut over to the other
3. Some problems are inherently distributed like inter bank transfers
4. To achieve security via isolation


[6.824]: https://www.youtube.com/watch?v=cQP8WApzIQQ&list=PLrw6a1wE39_tb2fErI4-WkMbsvGQk9_UB
[Prof. Robert]: https://en.wikipedia.org/wiki/Robert_Tappan_Morris