![Soma logo](https://github.com/JayPanoz/Soma/raw/master/assets/soma.png)

# Soma for Ulysses app

Soma is a blueprint for an ePub CSS compliant with all major Reading Systems (RS): 

- Adobe RMSDK;
- Readium;
- iBooks;
- Kindle.

**Please note Soma was specifically designed for [Ulysses App](http://www.ulyssesapp.com) and can’t be used for other software AS-IS.**

## Licence

**The MIT License (MIT)**

Copyright (c) 2016 Jiminy Panoz ([Chapal & Panoz](http://chapalpanoz.com))

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the “Software”), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

## Install Soma

1. Download this repo as zip
2. Unzip the downloaded file
3. Open Ulysses’ preferences
4. Go to “Styles” tab
5. Click “Add styles” button
6. Select “Soma.ulstyle” and import it as ePub theme

You can now use Soma when exporting to ePub.

## Goals

The aim of this blueprint is to provide with a stylesheet you can trust when exporting to ePub (from Ulysses app).

As a consequence, it relies on several principles: 

1. **Soma is retro-compatible:** you can load your EPUB3 file onto an ePub2-only compatible device/app and it will work;
2. **Soma should not disable user settings:** this is insanely tough but carefully chosen styles are doing the heavy lifting—type settings and “themes” (night modes) should be OK;
3. **Soma has to deal with RS’ overrides:** RS are overriding styles to guarantee a minimum level of UX, Soma overrides those overrides when need be;
4. **Soma shall cope with different types of books:** Soma was designed for both fiction and non-fiction books;
5. **Soma must provide with Kindle support:** given Kindle’s market share, especially in the US and the UK, support for KF8 and legacy Mobi7 is mandatory.

Those principles are the backbone of Soma and explain why, for instance, `line-height` is inherited from `body` or why there are proprietary Kindle media queries in there.

## Design Approach

First and foremost, it is important to remember **Soma was designed for Ulysses’ ePub output** and won’t perfectly work with other authoring software—it covers vanilla markdown though, so making it work with other apps is just about adapting and extending.

In order to solve many issues, a “progressive enhancement” strategy has been adopted: Soma was designed for ePub2, unsupported styles are declared only to inject some *cachet* in EPUB3 (e.g. links in small-caps). As for Kindle, KF8 is part of the global stylesheet, requiring only minimal alterations afterwards.

To sum (other) things up: 

- typesetting is based on Minion Pro, which is the default serif for Adobe’s RMSDK (again, market share);
- a modular scale (major second) has been used to achieve harmony and solve the “smartphone issue” a.k.a. small screens on which users may increase font size;
- vertical rhythm has been enforced to achieve harmony.
- a “mini reset” makes sure HTML5 block elements are displayed as they should in ePub2 RS—don’t get rid of it;
- declarations for `body` improve hyphenation + widows & orphans, they also provide with defaults all elements can inherit when need be (user settings issue);
- hyphens and page breaks have been taken care of for specific elements (e.g. headings);
- overall, typesetting has been prettified (e.g. paragraphs’ conditional `text-indent`, vertical positioning of `sup`, reset to roman/normal for italic nested in italic, &c.);
- horizontal rule is compliant with night modes;
- `figure` and `video` dimensions have been taken care of in order to guarantee they won’t overflow;
- `aside` has been styled so that the footnote be considered a footnote when displayed (only iBooks hides that since it is using it in a pop-up);
- Since mobi7 doesn’t support decimal numbers, all values are rounded for it.

Yeah, I know, Soma seems bloated compared to the default Ulysses themes but don’t forget it is dealing with a lot more RS. From my understanding, default stylesheets only deal with iBooks. Now, iBooks is ±10% of the US market and if you’re using Ulysses to write and publish your eBooks, this clearly leads to a missed opportunity.

Besides, there is no suggested rendering specified for eBooks—as is the case with HTML5 for instance—so that’s another issue Soma has to cope with. And I’m not even mentioning overrides…

To put it simply, Soma is a humble contribution to a better UX in all major RS, not just iBooks.

## FAQ

### Why the charset?

Shall you edit this theme, this charset will allow you to put non-ASCII characters in `content` using `:before` or `:after` pseudo elements. This comes handy since Mac OSX has a character palette in which double-clicking a glyph will add it in your document.

### Why the namespaces?

If you want to style using the `epub:type` attribute without a dirty “escaping character hack”, the epub namespace is required. 

The xhtml namespace may also be useful if you edit your ePub file manually and use tags like `acronym`, which is deprecated in HTML but not in XHTML.

### Any tips for headings?

When designing this stylesheet, I saw that Ulysses' developers decided that chapter title is `h1` so use that for… chapter titles. Use `h2` for section titles (inside chapters).

### Why are h5 and h6 left unstyled?

> If you feel the urge to reach for a heading of level 4 or greater, consider redesigning your document ([Tufte CSS](https://edwardtufte.github.io/tufte-css/))

I’m cool with the idea `h4` may be useful for very complex outlines (like in programming books) but that’s my personal limit. This is the reason why those two headings are left unstyled.

### Footnote call doesn’t work outside iBooks, is it normal?

Yep, this is normal as the output markup was designed for iBooks. Making it work would either require a change at the output level or a manual edit on your side afterwards.

If you want to do it yourself, here’s the HTML + CSS to use:

#### HTML

##### Footnote call

```
<p id="fnreturn-0001">Some text<a class="call" epub:type="noteref" href="#footnote-0001">(1)</a></p>
```

##### Footnote

```
<aside class="footnotes" epub:type="footnotes">
  <p class="footnote">(1) <span class="inline-iBooks" epub:type="footnote" id="footnote-0001">Footnote</span>&#160;<a class="callback" href="#fnreturn-0001">↵</a></p>
 </aside>
```
 
#### CSS
 
```
 .footnotes {
	margin: 3em 0 0 5%;
}
.footnote {
	text-indent: 0;
	font-size: 0.875em;
	line-height: 1.7142857143;
}
.inline-iBooks {
	display: inline;
}
.call {
	color: inherit;
	-webkit-text-fill-color: inherit;
	text-decoration: none !important;
	border: none !important;
	font-weight: normal;
}
.callback {
	font-family: Symbol, Arial, sans-serif;
	font-weight: bold;
	text-decoration: none !important;
	border: none !important;
}
```
 
To clarify: 

- we’re not using a `<sup>` for the footnote call as we need a target “tappable” enough on RMSDK-powered devices/apps (eInk readers for instance), hence the number between `(` and `)` to make it bigger.
- we need to link the call and the footnote (using `id`, `href` and the callback);
- because the footnote is not a direct child of `aside` (there’s a `p` and a `span` in between), **iBooks will display this footnote** at the end of the XHTML document—you may want to add a horizontal rule before, as in print books.

On the one hand, **if you’re publishing in the iBookStore only, changing the markup is your call.** You’d probably be better off with Ulysses’ default output. 

On the other hand, **if you want to publish anywhere else, you got no choice.** Footnotes’ UX will be abysmal as they won’t work properly. Then some readers will say that you don’t know shit because you can’t hyperlink. Finally you’ll get a bad rap and it’s game over.

### Converting to Kindle

Use [Kindle Previewer](http://www.amazon.com/gp/feature.html?docId=1000765261). Seriously. Your Kindle readers will enjoy a better UX than with a Kindle Mobi generated using some third-party software—and it will be easier to maintain if you edit your epub files manually.

### Editing Soma

Oh OK. Yeah, look, I didn’t document it very well… ¯\_(ツ)_/¯

But documenting a CSS for ePub is a really tough job—basically, comments everywhere leading to bloat, BLOAT, **BLOAT!** So let’s say that if something is surprising to you, don’t change it.

However, I may give some pieces of advice for some really important stuff.

- `-webkit-text-fill-color` is allowing the override of iBooks’ `a` default color. Now, iBooks got 4 themes (white, sepia, gray, black) and the value of the default color is modified dynamically based on the theme currently in use—in order to meet WCAG 2.0’s contrast ratio. In other words, don’t override and stick to default if you want to use color for links. As far as I can tell, there is no color meeting this WCAG requirement for all 4 themes.
- Do **never** declare black or shades of gray using `color` for text: some apps won’t invert this value when the user enables night mode and, as a result, they’ll get dark text on a dark background, making the text unreadable. If you really need black, use `color: inherit`.
- Do **never** declare `font-family` for `p` or `li` and other critical bodycopy element as it will disable the typeface setting for users in a shitload of apps and devices. Let the cascade do its job, making those elements inherit from `body`.
- Don’t rely on the `html` selector. Please note RS usually declare `text-rendering: optimizeLegibility` when it doesn’t have issues (rendering or performance) so do not impose it on users—especially as there are wild bugs out there. 
- Don’t rely on `text-transform` and `font-variant`, pseudo-classes and -elements for “critical styling”, e.g. headings, strong emphasis, list style type, thematic break (`hr`), &c. This is not supported in RMSDK.
- If you have snippets of code in your eBook, you’d better embed a monospace font in your ePub file. Some devices/apps don’t have a default one and will render code as serif or sans-serif.
- Be insanely cautious with value `em` for margins and paddings, especially left and right; if the user increases font-size, margins will increase accordingly. As a result, you get bigger text in a smaller container. This is the reason why I am using values in `%` (computed based on `width` of container).
- Styling of HTML5 elements (e.g. `aside`, `figure`, `section`, &c.) relies on the mini reset. Don’t ever get rid of this reset, please, ePub2-only RS being still popular worldwide.
- If you change the modular scale (i.e. `font-size`), all line-heights and margins have to be recalculated if you want to keep vertical rhythm.
- Don’t use decimal values in `@media amzn-mobi{}`, KindleGen converter will round them quite harshly. **Please also note Mobi7 supports ± HTML 3.2.**
- Cover img styling in ePub is crap. **Design your cover with a 16:9 aspect ratio and you should be safe.** 16:10 or 4:3 and you’re screwed–unless you modify `img.cover` styles, that is.
- Whatever you do, don't leave the proprietary Amazon media queries (`amzn-kf8` + `amzn-mobi`) empty: **it will crash RMSDK.** CSS comments won’t fix this issue.

### How do you manage your stylesheets?

I’m basically using [LESS](http://lesscss.org) so that I can automatically compute vertical rhythm in `em` based on body’s `font-size`, `line-height` and a predefined modular scale.

This in-house tool is currently being extended into a CSS framework for eBooks. More about that soon.

For the record, the backbone of Soma’s styling was generated using v.0.66.6 of this framework.

### Is Soma offerring some sort of Universal Support?

Alright, you want to know if this stylesheet is likely to save your arse absolutely everywhere? 

I’m afraid I can’t guarantee that since a lot of ePub apps don’t even fully implement ePub2 specs–I’m looking at you, Android apps… Die, Die, DIE!

Listen, some apps don’t even support the `hr` tag because their developers don’t give a damn about structure and semantics. Others don’t even support CSS and I’m not kidding.

So if some readers complain, tell them to use another app, a better one. And if you want to publicly shame bad apps, rest assured I won’t stand in your way–Yeah, that guy far away, cheering you on, that’s me.

### Issues & feature requests

**If you got issues, file them.** To the extent possible (see previous question), I’ll do my best to fix them.

**If you got feature requests, fork Soma and make it happen.** Soma is covering all stuff output by Ulysses and I won’t add features for editing afterwards. 

Disclaimer: got an awful amount of work to deal with like, say, making eBooks to pay my rent + developing the “CSS framework for eBooks” that should have been launched like one year ago. In other words, my spare time is scarce.

**Nota Bene: all disrespectful messages are belong to “is:issue is:closed”.**

### Useful links for eBook Production

eBook Production is a little bit different than web development. So here are some links that could prove useful…

- [MobileRead Forums](http://www.mobileread.com/forums/)
- [Twitter’s #eprdctn](https://twitter.com/hashtag/eprdctn)
- [ePub Zone](http://epubzone.org)
- [my blog](http://jiminy.chapalpanoz.com) if you speak French

### Will you push Soma to Ulysses’ Style Exchange?

Yeah, probably. But only when I’m sure everything is OK and there are no critical issues with it.

### Are companion themes (e.g. PDF, HTML, &c.) planned?

Nope. For lack of time and courage, I’m not planning to do that. But again, this is just a fork away….

### How to contact you? 

OK, look, I’m a freelance so I must deal with so many things in so little time… Having more stuff to deal with would probably kill me. 

Here are 3 simple rules to follow:

- if you have problems using or editing Soma, use Github issues;
- if you want to say hello—or maybe thank me?—, use [Twitter](https://twitter.com/jiminypan);
- **only if** you want to talk business, use the contact form on my website.

Thank you in advance.
