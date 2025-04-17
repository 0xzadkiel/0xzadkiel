
---
title: "Open Source Intelligence"
published: 2025-04-18
image: "./cover.png"
tags: ["OSINT"]
category: "Guide"
draft: false
---



Open source intelligence is a skill that ones develop over time . The thought process is what's important not the tools or the website at disposal . Osint is about gaining a piece of information like a piece in a puzzle and discovering the whole puzzle and it's pieces . 
We might get a picture or a username etc ... Key is to find any reliated information  we can find on that piece of infomration and chain it together to make sense of things . Everything is out there we just have to look at the right place with the right eye to find it . 

+ [A brief History of Open source Intelligence](https://www.bellingcat.com/resources/articles/2016/07/14/a-brief-history-of-open-source-intelligence/)

## Sock Puppets
![[/images/2025-04-18_00-58.png]]
Sock puppets is a terminology used for a Fake identity created on the internet . It can be social media accounts for example . Key is to make them look as authentic as possible so they can be used later for compeigns . 

Resources : 
+ [The Art Of The Sock](https://www.secjuice.com/the-art-of-the-sock-osint-humint/)
+ [Creating an Effective Sock Puppet for OSINT Investigations – Introduction – Jake Creps](https://web.archive.org/web/20210125191016/https://jakecreps.com/2018/11/02/sock-puppets/)




## Search engines
![[/images/Seer-Interactive-Crawling-3.webp]]

Search engine is the best tool for any OSINT expert . Search engines are not as plain as people use it . We have functions and filters that we can use that can sort the results according to our desire . This filters can be based on date,heading,title or even text . 
Different search engines will give you different results for the same search so it's always a good idea to use multiple search engines . 

Few Popular Search engines : 
+ [Google](https://google.com) 
+ [Yandex](https://yandex.com)
+ [Baidu](https://baidu.com)

Some of the common Filters in the search engines are 
+ Site:  (Will only show results from a specfic site of choice)
+ fieltype:   (Will only show results with a filetype of choice )
+ -word (remove a specfic word reliated sites from your results)
+ intitle: (Will show results where it matches the title of choice)
+ inurl (where the url matches your definiation )
+ intext ( where text matches your criteria )

We also have search engines that are not meant for websites but rather IOT devices and internet of things . We can search for Servers , Power plants , Industrial control system and so much more with them . 

Such search engines are : 
+ [Shodan](https://www.shodan.io/)
+ [Fofa](https://en.fofa.info/)
+ [Censys](https://search.censys.io/)



These search engines have  a similar layout we can search for Ip-address and few other key components and filter results based on vulnerabilities , Open ports , Running services , Location and etc ... 

Example : Integrated Dell Remote Access Controllers in USA that are on the internet .
![[Pasted image 20250417234841.png]]




## Social media
![[/images/2025-04-18_00-47.png]]

Social media is a Gold mine of information . People post stuff and forget about what impact that piece of information can have on there lifes . A careless user is his worse enemy . 
This can be a person posting name of there dog or fav basketball team  and looking at recent events we can see that most people use combination of those and few other basic informations on them in there passwords which can lead to someone cracking there passwords . Or creating a phishing link according to there interest


We'll cover few popular social media sites and how we can use them for our osint 


**Twitter**
Twitter is by far one of my fav platform when it comes to osint . People Post about there opinions and usually follow people who are like minded and approve of there opinions on things which reveals alot already . But twitter also has some nice search filters that we can use to gather information on a user . 

- **`"exact phrase"`** – Find tweets verbatim (e.g., `"my birthday is"`).
- **`from:@username`** – Every cringe tweet they’ve ever posted.
- **`near:city`** – Geolocated hot takes (pair with `within:15mi`).
- **`filter:images`** – Find their face, their cat, or their lunch.


As you can see twitter gives us alot to play with without even forcing us to follow that person so it's all passive . 


**Facebook**
Facebook also has some nice search terms included in there search bar . These are filters such as people from specfic location , Degree , Schools , Events  etc ...

We can use the facebook search bar itself or we can use this site :
+ https://intelx.io/tools?tab=facebook 




**Tiktok & Instagram**
There isn't much we can do on these platforms if a profile is private . We can try sending them request and if they accept them we can then see who they follow and there content . But we still get the Profile picture they use . There username and there bio which might include some other information that can lead us to something more interesting . 
Instagram search bar also dosen't have any search filters We have general # to see a specfic trend filter and @ to see specfic usernames anmd "" to get the exact result if it exists . 



**Reddit**
Reddit is also very uesfull due to it's history features . People usually forget about the posts the commented on or what they posted in a thread . but a simple go through on there profile can show us there comment and post history . It is very valuable to us and i'll explain it to you by an example  because why not . 

Example : Let's say we have a username that we want to perform osint on and we put it in oone of our site to see if it exists on certian sites and we got a positive match for reddit and instagram . There instagram is private however  We visit there reddit profile . We can also confirm that it's them based on there interests which we can get the idea of based on there thread history or maybe they have the same profile picture as on instagram or bio . We can see how they type , When they are online and what communities they are part of which can make one with enough effort and time very close to that human being . It can be a stalker which now joins the communities that you are part of showing interest and later adding you as a friend and start to chat with you based on interest he knows you have because of your thread history and your opinions on stuff based on your comment history 

**Snapchat**
Snapchat dosen't have much to it however we have maps.snapchat.com which can filter snaps based on a certain given area . To see snaps from a certain user we have to add them 



## Physical OSINT
![[/images/2025-04-18_00-52.png]]
Let's say you are a Pentester or a Red-teamer and you have to physically break into an organization . We should know few things before we proceed such as the layout of the building . Security posture of the company . Probable holes and how to exploit them . 

Example : Let's say we are performing OSINT on an organization and from doing osint on there social medias we found few of there employee and one of them posted a badge and we also searched for near  by restaurants and coffee shops and we found few reviews from the employees of that company about the coffee shop . Now we can craft a Phishing email or we can proceed with our physical osint and perhaps get a RFID scanner and go to that coffee shop ourselves and see if we can get some badges and break into the building that way . From Looking at google maps we already know the layout of the building and we know how the employees usually dress from certain departments and from social media we know who is the head of that department and who "Our coworkers" will be . 




## Image OSINT
![[/images/Image.png]]
There is a popular saying " Image Speaks a thousand words" and there is truth in that . Images can reveal alot to the right eye . People are to careless about what they post on the internet . 

There are 2 major things an an image 
+ Content
+ Metadata 

Metadata is usually removed when a image is posted on a social media platforms , However if we get a picture otherwise we might have a chance on getting our hands on this metadata. This is what the device includes in the image . It can be things such as GPS Coordinates , Device Name and Model that took the image etc ... 

On the other hand image itself can reveal alot of information. We can put the image in reverse image search to see where it's been posted before we can focus on a certian building , cars plate numbers , boards and signs  in the background and see where it's located at to get the location of where the image was taken . Recently a game called Geoguesser as been really popular if you wanna polish your skills on guessing locations from images .
+ [GeoGuessr](https://www.geoguessr.com/)



---
## Resources 

**Email**
+ [Find email addresses and send cold emails • Hunter](https://hunter.io/)
+ [Email Finder: Free 50 Verified Email Addresses - VoilaNorbert](https://www.voilanorbert.com/)
+ [Email Checker - Verify Email Address Online](https://email-checker.net/)


**Phone numbers**
+ [Phonebook.cz - Intelligence X](https://phonebook.cz/)


**Websites**
+ [wayback Machine](https://archive.org/)
+ [DNS Checker - DNS Check Propagation Tool](https://dnschecker.org/)
+ [ANY.RUN - Interactive Online Malware Sandbox](https://any.run/)
+ [Check if a Website is Malicious/Scam or Safe/Legit | URLVoid](https://www.urlvoid.com/)

**Usernames**
+ [Namechk - Username and Domain Name Checker - Search All Domain Names and User Names to see if they're available](https://namechk.com/)
+ [WhatsMyName Web](https://whatsmyname.app/)
+ [NameCheckup - Find Available Username](https://namecheckup.com/)
+ [Whitepages® - Official Site | Find People, Phone Numbers, Addresses & More](https://www.whitepages.com/)
+ [PeekYou - Fast People Search Made Easy](https://www.peekyou.com/)


**Passwords**
+ [DeHashed — #FreeThePassword](https://dehashed.com/)
+ [LeakCheck - Find out if your credentials have been compromised](https://leakcheck.io/)


**Flights**
+ [Wizz Air Malta - AirNav Radar Flight Tracker](https://www.airnavradar.com)
+ [FlightAware](https://www.flightaware.com)

**Wireless Networks**
+ [WiGLE: Wireless Network Mapping](https://wigle.net/index)

**Sock puppets**
+ [(JPEG Image, 1024 × 1024 pixels)](https://thispersondoesnotexist.com/)
+ [Generate a Random Name - Fake Name Generator](https://www.fakenamegenerator.com/)


**ETC..**
+ [Who posted what?](https://whopostedwhat.com/)
+ [Flickr: Explore everyone's photos on a Map](https://www.flickr.com/map/)
+ [Wedding Registry Search and Website Finder](https://www.theknot.com/registry/couplesearch)
+ https://www.osinttechniques.com/osint-tools.html
+ [BGP.Tools](https://bgp.tools/)




## Suggestions
Learn scripting so you can automate stuff or heck use Deepseek or chatgpt to create scripts for you  . With time you will develop your own methodology and collection of scripts and tools that work for you the best . Never stop learning and if you are serious about OSINT . My personal recommendation is for you to consider reading this book . 

+ [OSINT Techniques  by Michael Bazzell](https://inteltechniques.com/book1.html)


