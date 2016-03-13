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

1. [Download the pre-release](https://github.com/JayPanoz/Soma/releases/tag/v0.9)
2. Unzip the downloaded file
3. Open Ulysses’ preferences
4. Go to “Styles” tab
5. Click “Add styles” button
6. Select “Soma.ulstyle” and import it as ePub theme

You can now use Soma when exporting to ePub.

## Goals

The aim of this blueprint is to provide a stylesheet you can trust when exporting to ePub (from Ulysses app).

As a consequence, it relies on several principles: 

1. **Soma is retro-compatible:** you can load your EPUB3 file onto an ePub2-only compatible device/app and it will work;
2. **Soma should not disable user settings:** this is insanely tough but carefully chosen CSS declarations are doing the heavy lifting—type settings and “themes” (night modes) should be OK;
3. **Soma has to deal with RS’ overrides:** RS are overriding styles to guarantee a minimum level of UX, Soma overrides those overrides when need be;
4. **Soma shall cope with different types of books:** Soma was designed for both fiction and non-fiction books;
5. **Soma must provide Kindle support:** given Kindle’s market share, especially in the US and the UK, support for KF8 and legacy Mobi7 is mandatory.

Those principles are the backbone of Soma and explain why, for instance, `line-height` is inherited from `body` or why there are proprietary Kindle media queries in there.

## Design Approach

First and foremost, it is important to remember **Soma was designed for Ulysses’ ePub output** and won’t perfectly work with other authoring software—it covers vanilla markdown though, so making it work with other apps is just about adapting and extending.

In order to solve many issues, a “progressive enhancement” strategy has been adopted: Soma was designed for ePub2, unsupported styles are declared only to give some *cachet* to EPUB3 (e.g. links in small-caps). As for Kindle, KF8 is part of the global stylesheet, requiring only minimal alterations afterwards.

To sum (other) things up: 

- typesetting is based on Minion Pro, which is the default serif for Adobe’s RMSDK (again, market share);
- a modular scale (major second) has been used to achieve harmony and solve the “smartphone issue” a.k.a. small screens on which users may increase font size;
- vertical rhythm has been enforced to achieve harmony.
- a “mini reset” makes sure HTML5 block elements are displayed as they should in ePub2 RS—don’t get rid of it;
- declarations for `body` improve hyphenation + widows & orphans, they also provide defaults that all elements can inherit when needed (user settings issue);
- hyphens and page breaks have been taken care of for specific elements (e.g. headings);
- overall, typesetting has been prettified (e.g. paragraphs’ conditional `text-indent`, vertical positioning of `sup`, reset to roman/normal for italic nested in italic, &c.);
- horizontal rule is compliant with night modes;
- `figure` and `video` dimensions have been styled to guarantee they won’t overflow;
- `aside` has been styled so that the footnote be considered a footnote when displayed (only iBooks hides `aside` content since it is using it in a pop-up);
- Since mobi7 doesn’t support decimal numbers, all values are rounded for it.

Yeah, I know, Soma seems bloated compared to the default Ulysses themes but don’t forget it is dealing with a lot more RS. From my understanding, default stylesheets only deal with iBooks. Now, iBooks is ±10% of the US market and if you’re using Ulysses to write and publish your eBooks, this clearly leads to a missed opportunity.

Besides, there is no suggested rendering specified for eBooks—as is the case with HTML5 for instance—so that’s another issue Soma has to cope with. And I’m not even mentioning overrides…

To put it simply, Soma is a humble contribution to a better UX in all major RS, not just iBooks.

## FAQ

See [Wiki](https://github.com/JayPanoz/Soma/wiki)
