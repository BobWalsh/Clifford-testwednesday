---
layout: post
title: "WordPress to Gridsome via Markdown in 2019 explained."
date: 2019-10-23 15:01:35 -0600
image: '/images/47hatsnow2.png'
tags:   [Gridsome, Development]
---

## Table Of Contents:

- [Table Of Contents:](#table-of-contents)
  - [Where I started from<a name="Where I started"></a>](#where-i-started-from)
  - [Overview of the Challenges<a name="Overview of the Challenges"></a>](#overview-of-the-challenges)
  - [Creating the gridsome site<a name="Creating the gridsome site"></a>](#creating-the-gridsome-site)
  - [Hosting on Netlify<a name="Hosting on Netlify"></a>](#hosting-on-netlify)
  - [Converting to markdown, Part 1<a name="Converting to markdown, Part 1"></a>](#converting-to-markdown-part-1)
  - [Converting to markdown, Part 2<a name="Converting to markdown, Part 2"></a>](#converting-to-markdown-part-2)
  - [The Result: my blog in gridsome<a name="The Result: my blog in gridsome"></a>](#the-result-my-blog-in-gridsome)

### Where I started from<a name="Where I started"></a>

So in this post, I'm going to outline the process of moving a large WordPress blog to [gridsome](https://gridsome.org/).

[47hats](https://47hats.com) has some 700+ posts and about as many images, going back to 2006: this is not some demo WordPress site!

While it's debatable that anyone in their right mind would go back and read my posts from say, 2010, Google likes seeing that a site is a body of work, not just a few marketing posts thrown up to try and produce SEO magic.

### Overview of the Challenges<a name="Overview of the Challenges"></a>

I faced (at least) 4 questions:

1. Could I get all my content out of WordPress and free from the clutches of at least 3 "helpful" WP page editors, including WP's latest Gutenberg?
2. Once I got that content free, could I convert it to markdown, preserving links, links to images, tags, and the like?
3. Assuming I could do 1 and 2, should I then import these files into a headlessCMS to make it easier to edit and maintain them, or, stick with markdown?
4. Which Static Site Generator should I go with? I'd worked with Jekyll and added a blog to my Ruby on Rails project LearnShortcuts, had played around with Hugo, and was in the process of teaching myself Vue. I decided to stick with Vue because I hope to retool myself as a Vue developer. I considered [Gatsby](https://www.gatsbyjs.org/) but did not want to mix learning Vue with (relearning) React.

So once I decided on Vue, which Vue framework should I use? There's at least 3 worth consideration:

- [VuePress](https://vuepress.vuejs.org/) - the "official" Vue documentation SSG was a strong contender. It offers out of the box: a number of markdown extensions, a framework ready for markdown files, and an extendable plugin system. But, in working through the demos I was stymied trying to go with anything but the default theme. If I knew more Vue, perhaps the documentation would have been adequate, but for me, this was a showstopper.
- [Nuxt.js](https://nuxtjs.org/) - I like Nuxt, it reminds me of the good parts of Ruby on Rails. Convention over configuration and a complete framework plus its 1st-class support of static site generation. Nuxt comes with excellent docs (which I supplemented with an extremely excellent Udemy class by [Maximilian Schwarzmüller](https://www.udemy.com/share/102092CEIdeVs=/)).

But Nuxt is a comprehensive framework - lots to learn, lots to go wrong for a [vue] newbie. This was complicated when I went down another rabbit hole: [Vuetify](https://vuetifyjs.com/en/). After all, I want my site to look good, and Vuetify seemed to deliver. But attempting to bolt on Vuetify to either of this great Nuxt templates, **[awake-template](https://github.com/danielkellyio/awake-template)** or **[nuxt-markdown-blog-starter](https://github.com/marinaaisa/nuxt-markdown-blog-starter)**, did not work for me.

Again - my bad, or at least my lack of knowledge. Still, I'd committed to leaving WP and time was a-wasting! Was there a solution? Or would I have to settle for something less than I wanted or (gasp!) build this in gatsby or React proper?

- Then I found [gridsome](https://gridsome.org/). This was the first Vue framework that really paid attention to SEO in its design, not just as a npm package addon. "Gridsome sites loads as static HTML before it hydrates into a fully Vue.js-powered SPA. This makes it possible for search engines able to crawl content and give better SEO ranking, and still have all the power of Vue.js." It's documentation was excellent, there were [starter kits](https://gridsome.org/starters/) using a variety of themes and GraphQL meant that to me that in the future I could add content sources, and place a bet on GraphQL to supplant RESTful routing.

### Creating the gridsome site<a name="Creating the gridsome site"></a>

Building a site in gridsome is almost absurdly easy. Start with a Starter Kit. Follow directions. Done!:

I picked the `gridsome-starter-blog` starter kit, because a) along the way, I had decided against a headless CMS, b) I wanted a good looking site from the get-go, and this starter kit offered one, c) I liked the dark mode widget, and dark mode in general, and while my assistant code-writing cat helper prefers white backgrounds, I'm finding a lot less eyestrain in a Dark Mode digital world.

![Audrey tanning](./images/Audrey_tanning_use.jpg)

So,

```bash
npm install --global @gridsome/cli

gridsome create my-gridsome-site https://github.com/gridsome/gridsome-starter-blog.git
//(my-gridsome-site is your new site.)

cd my-gridsome-site

gridsome develop //(to start local dev server at http://localhost:8080)
```

Boom. that's it, and I had at least the bare-bones version of my 2019 47hats site running on localhost. it looked like this: [https://gridsome-starter-blog.netlify.com/](https://gridsome-starter-blog.netlify.com/).

### Hosting on Netlify<a name="Hosting on Netlify"></a>

I've gotten somewhat proficient using [Netlify](https://www.netlify.com/) to host SSGs, and found only 2 gotchas worth talking about when it came time to set up Netlify as my host for 47hats.com:

1. With this SSG, you want to let Netlify digest your entire repo, then rebuild the site. So that means the Build Command is `gridsome build` and the Publish directory: `dist`.

2. Connecting your domain with a Netlify domain. The key gotcha here is if you are using a URL that is not the Netlify-generated URL, Netlify is going to take over hosting your site's DNS. You're going to want to a) copy all of the DNS records, including **MX records that govern email**, before you make the switch, and then [add those records back into Netlify's DNS](https://docs.netlify.com/domains-https/netlify-dns/dns-records/#supported-record-types). Otherwise, you will be shedding a lot more than just whatever service you were using for your WordPress site.

### Converting to markdown, Part 1<a name="Converting to markdown, Part 1"></a>

Running my site on Netlify was the easy part: now to deal with those 700+ posts.

The first step is to export from WordPress your content. Sign into your WordPress instance, go to Tools, then Export. Chose "All Content", and watch a .zip file get downloaded. This will export in the quaint format XML all of your posts, pages, comments, categories, and tags.

The zip file contains two objects: the XML file containing all of the text of your posts and metadata about those posts and a folder of all graphic files referenced by your posts: cover art and featured art.

After much googling and a fair amount of trial and error, I found this repo that got most of the hard work done: [wordpress-export-to-markdown](https://github.com/lonekorean/wordpress-export-to-markdown). I'd recommend forking it right now, then clone your repo. This is a keeper - it works.

> But wait, it doesn't say a word about Vue! **markdown is markdown** whether it's created for Jekyll, Huge or gridsome. They all share the convention of a `frontmatter` block at the top of the file, then the contents itself.

I found (through some more trial and error) that the best processing of my XML file came from using these three flags:

- **--postfolders=false** This will save and reference all the images to a single folder, /images. This matches what gridsome expects. This means all those URLs pointing to an image on your old service get magically changed to a relative URL and you won't go blind doing manual edits for the next year.
- **--addcontentimages=true** Super useful! This will scrape your posts for external images and attempt to download and reference as many of them as it can. That content may no longer be on the web, but if it is, this script will get it.
- **--prefixdate=true** This will prefix each post's filename with the date it was posted. Extremely handy for the next step.

One last thing about this great repo by [LoanKorean](https://github.com/lonekorean), AKA Will Boyd. You either need to change line 27 of `index.js` to the name of your XML file or change the filename to the default, `export.xml`.

### Converting to markdown, Part 2<a name="Converting to markdown, Part 2"></a>

After the conversion had run its course, there were still 3 inconsistencies between the format of my markdown files and what gridsome expected. I imagine that at some point in my Vue learning journey, I could hack away these inconsistences, but I found it easier to just [power edit](http://www.macdrifter.com/2012/08/bbedit-multi-file-find-and-replace.html) all files back to 2017 to start.

1. "true" vs true. gridsome expects in the frontmatter date: datevalue`, not`date: "datevalue". Posts with date in quotes will silently fail to render and disappear from your blog. So using [BBEdit](https://www.barebones.com/products/bbedit/), I stripped the double quotes from the frontmatter entirely.
2. Where's my cover-image? [wordpress-export-to-markdown](https://github.com/lonekorean/wordpress-export-to-markdown) exports `coverImage: "tab-groups-4.gif"`, but gridsome expects `cover-image: ./images/tab-groups-4.gif`. Again, BBEdit to the rescue, replacing "coverImage" with "cover_image", adding ".images/" to each filepath and then getting rid of double quotes entirely.
3. For whatever reason, several posts got "null" for dates. These were easy enough to manually fix, or just outright delete.

### The Result: my blog in gridsome<a name="The Result: my blog in gridsome"></a>

I converted the last 15 posts, just to make sure my BBEdit process worked. It did! I plan to do the rest this weekend, and add some features like search, visible tags to pick, and building out my Hire Me and Books page. (I've already enabled pagination). Converting a site is never done, but I'm fully confident that this is easy to do.

I've shared this before, but it still blows my mind:

![47hats now](https://thepracticaldev.s3.amazonaws.com/i/3ec5nwnso2wurog97xv4.png)

Now I have:

1. A highly performant blog where once I had a bloated monster,

2. My content in a format that is device, framework, and app independent: for example, I'm writing this post in [Typora](https://typora.io), my current preferred markdown editor. If I decide to go back to MacDown, or onto some other markdown editor (Maybe Bear), I can do that without changing anything.

Thanks for reading this rather long post: please tell me how it goes for you in comments here at [dev.to](https://dev.to), or reach out to me developer-to-developer at [bob.walsh@47hats.com](mailto::bob.walsh@47hats.com).
