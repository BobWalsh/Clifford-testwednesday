---
layout: post
title: "Why I switched to the JAMstack for my blog"
date: 2019-10-17 15:01:35 -0600
cover-image: 'images/47hatsnow2.png'
tags:   [JAMstack]
---

I've been running my "personal brand" blog at [47hats](https://47hats.com) since 2005 on one platform or another; for the past decade in WordPress.

I _hate_ WordPress.

I'm convinced that all that never-ending messing with plugins, theme configurations, broken wp installs and the like took all the joy out of blogging for the first generation of bloggers.

And performance **sucked**:

![This score sucked](images/47hatsthen2.png)

But the imperatives for blogging haven't gone away: we still need a public way to connect to people, to show what we are (mostly) good at, to register as a "thought leader" in whatever we are passionate about.

### Getting on the JAMstack movement

I've seen a lot of various movements come and mostly go in the tech industry: Windows OS (don't judge) at one time was revolutionary. Then, for me, there was Ruby on Rails. Then .js everything, React, Vue, and on and on. The difference between a tool (like webpack, gulp, etc.) and a movement is two things: people and affordances.

About a year ago after listening to [Wes Bos and Scott Tolinski](https://syntax.fm/) raving about [netlify](https://www.netlify.com/), I decided to give it a try with a demo Jekyll example blog. I sat there gobsmacked for 10 minutes because I had never, ever, seen such an easy deploy resulting in such a performant site.

I started noticing how many interesting people were moving over to the JAMstack and how many startups were springing up in the space: this is a movement, not a shiny new object.

### Making the switch, get to the punchline

So last month I decided instead of hosting my WP site in WordPress on Siteground (the best of the WP hosts, IMO) for another year, I'd see if I could stand up my blog in Vue on Netlify.

I rapidly descended down several rabbit holes: first Nuxt, then VuePress, then Vuetify, then [gridsome](https://gridsome.org/). Gridsome is an awesome framework for someone who needs a home for their blog, with a few pages and wants to get it done **now**.

Last night I threw the [dns] switch, and today my site looks like this:

![47hats not sucking](./images/47hatsnow2.png)

My JAMstack journey is in its early days; HubSpot and Rails still pay the bills. But there are some amazing opportunities to create value that's performant, fun to build and looks good everywhere: I hope to find my place in the JAMstack Revolution.
