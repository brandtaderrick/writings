# Software Short Story - OAuth2.0

In ye olden days(2006), websites used to ask web navigators for their username and password. I'm not talking about your typical sign-on into Gmail. I'm talking about dangerous love triangles between multiple web applications.

Beth, an early explorer of the web, had an amazing idea: "I want to use this new app called Twitter!" So, she created an account and started tweeting away. One day, Twitter, being a fast growing and prominent company, decided "it is a good idea to interact with other web apps." Thus, Twitter developers began their keyboard maelstrom. 

Beth, now a user of both Gmail and Twitter, was intrigued when Twitter released a brand new feature interacting with Gmail contacts. She logged on to try using it. Before she could, Twitter asked two things from her- her Gmail username and her Gmail password. Twitter, behind the scenes, used Beth's credentials, acquiring her Gmail data and braiding it into their application. Voila! A groovy fishing net of integration.
There was just one problem. Twitter was likely to store Beth's username and password on their *own* servers, **not** Googles, and in plaintext! Additionally (there are two problems actually), Twitter now, theoretically, had access to all Beth's data, even though only her email contacts were needed for the feature. Beth willingly turned over her username and password to Twitter, and that got her a) cool feature; and b) a whole lot of cyber-vulnerability. 
Not a great tradeoff. 

You needed three things for this love triangle: 
1. A user
2. A web application; and 
3. A separate web application with some intriguing features

The intention was pure - "I want to use both of these apps and have them do cool things together." However, the initial implementation demonstrated security vulnerabilities and personal privacy concerns. 

A problem was evident, so a bunch of nerds got together to do what nerds do - deliver a solution. **Boom, OAuth was born**. 

Unfortunately, as the story goes, this particular group of nerds did not play well together. And rather quickly, everything went to hell inside a flowery hand basket. Nevertheless, deep beneath the rubble, a perfectly intact launch pad was found unscathed. A launch pad later utilized by the rocket known as OAuth2.0

The aforementioned ups and downs of this group of nerds led to a new kind of server, an *OAuth* server. An OAuth server's job is very similar to a web server's job. At the end of the day, like a web server, he sits there and listens, ad infinitum, without emotions, friends or boredom.
What changed, however, is this - the server (who we will affectionately call Mr. OAuth) was now positioned as *the helm* of authentication and authorization. Mr. OAuth, this friendless lump of metal, was charged with confirming Beth is indeed Beth, then immediately screeching into the void what Beth is permitted to do, sort of like a tyrant. Years of isolation and alternating current has given Mr. OAuth a sort of God complex. 
As it turns out, this is good! Because you can add Mr. OAuth to your chaotic love triangle, creating a stable, righteous love square. 

Instead of asking Beth for her Gmail username and password, you now redirect her to an external page where she logs in (this is your first interaction with Mr. OAuth). After a successful login, Beth is redirected back to the application she just left (your second and final interaction with Mr. OAuth). Quietly, and without Beth's knowledge, the second redirect gave Twitter what it needs to successfully fetch Beth's contacts from Gmail's servers. Problem solved - no more security and privacy concerns.

That's probably not incredibly memorable, however, so here's a short story that almost perfectly maps on to this situation. Have a read while you are using sites like OAuth 2.0 [playground](https://www.oauth.com/playground/) to learn. 

## Short Story
Imagine Beth inside Twitter's luxurious HQ. She drinks coffee, she chats with you, and she loves the Gmail contacts idea. She wants to be a part of it. You say no problem, I'm a Twitter developer, we can do it. The both of you exit Twitter's glass door's and walk across the street to a big, menacing gray building.

You and Beth enter and are immediately surprised by the powerful art deco architecture. You see old timey black steel bars to your left. A man seems to be working, so you and Beth walk up to him together. Beth introduces herself with "Hi I'm Beth!"

Mr. OAuth, sat behind a mahogany ticket counter, looking grumpy and wearing a stained Google polo, checks his clipboard. After a glare that looks slightly derogatory, he asks "do you have any identification?"

Beth hands over her driver's license. Mr. OAuth scans the 3x2 inch plastic card and confirms Beth is in fact Beth. He rubber stamps his papers and hands you a perforated light green ticket. "Authorization Code: xi234slkdfjT," it reads. "Use this at the next window," he grumbles. To your right, you shuffle over to the adjacent counter and extend your ticket stub toward Mr. OAuth, who apparently does the job of two people.

Mr. OAuth squints at you for a few seconds, snatches your green ticket, and clacks away at his typewriter. With his nose pointed downward, his pupils slowly shift upward until he makes eye contact for one awkward second too long - "they match." Without any further explanation, Mr. OAuth hands you another perforated ticket. This time, it's double sided and salmon colored with "Token" printed on the front. Calmly, you turn over the new material and see "access_token: xy23f9vfr, refresh_token:298f2LS" on the back. 

Perplexed, you glance up at Mr. OAuth, hoping for more directions. Nearly (but not nearly) impressed at your stupidity, he leans and points his index finger over your shoulder. Behind you, another ticket counter.

Disillusioned by everything that has just happened, you spin around and walk up to the next ticket counter while praying that Mr. OAuth does not also work here. An employee behind black steel bars greets you with a nametag that says "Google Consultant, Mr. Aypeeigh." Relief ripples over you as he seems multiples less disgruntled than Mr. OAuth. Perhaps your expectations have been lowered. He eyes your perforated ticket. You (and not Beth) hand it over and say "I'd like a copy of Beth's photos please." He looks at the back of the ticket and says "Your authorization code works, but it looks like she guaranteed you would be given her contacts and nothing more. Shall I try something else?" A little embarrassed, you realize you've made a simple mistake and ask for her Gmail contacts. In the space of a wink, Mr. Aypeeigh suddenly holds a stack of papers. He transfers them through the ticket window along with the salmon ticket stub placed neatly at the top of the pile. Mr. Aypeeigh waves and finishes with "thank you for visiting Google headquarters, please come again soon."

Happy to finally be out of that bureaucratic spider web, you exit the building and walk back inside Twitter's glass skyscraper, Beth's contacts in hand. Your hard work and social interactions have made it possible for Beth to enjoy the feature she wanted so badly.

The end.
