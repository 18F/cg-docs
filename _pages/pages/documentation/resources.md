---
title: Additional resources
permalink: /pages/documentation/resources/
layout: docs
navigation: pages
sidenav: pages-documentation
redirect_from: 
  - /pages/using-federalist/resources/

---

## Working with markdown

Markdown is a plain-text simple markup language designed to be readable for
content editors. The syntax is easy to learn and is also used on GitHub.
Markdown is used to format the content on your website without having to read or
write HTML.

GitHub has an excellent guide for [mastering
markdown](https://guides.github.com/features/mastering-markdown/). Here's
a sample of what it looks like:

```
# This is a top-level h1 heading

This is just a normal introductory paragraph.

## This is a second-level h2 heading

*This text will be italic.*
_This will also be italic._

**This text will be bold.**
__This will also be bold.__

_You **can** combine them._
```

And this is what the above markup looks like when it is rendered on a web page:


> # This is a top-level h1 heading
> 
> This is just a normal introductory paragraph.
> 
> ## This is a second-level h2 heading
> 
> *This text will be italic.*
> _This will also be italic._
> 
> **This text will be bold.**
> __This will also be bold.__
> 
> _You **can** combine them._

You can practice with the [Markdown helper](https://markdown-helper.18f.gov/).


## Working with YAML

YAML is a markup language used for configuration of your Federalist site.
Variables defined in your YAML configuration can be used within your site.
Comments are ignored by Federalist. Indentation is important to keep related
options together. We recommend using space characters instead of tabs for
nesting configuration options.


```
# This is a YAML comment. It is ignored by Federalist.

# This is a variable "title".
title: "This could be the title of your site"

# This is an object "agency_contact" with
# properties "phone" and "email".
agency_contact:
  phone: "202-555-5555"
  email: "info@agency.gov"

# This is a list
list_of_departments:
- "Department of A"
- "Bureau of B"
- "C Administration"

# You can define any variables you like with YAML.
```


## Working with front matter

Front matter contains YAML configuration variables on a per-page basis. Some
variables are important to Jekyll like `layout` or `permalink`. You can also
include your own variables that can be referenced in your page content and
layouts. Learn more about [Jekyll front matter](https://jekyllrb.com/docs/frontmatter/).

```
---
# Front matter is always at the top of your page fenced by the
# three dashes (---).

title: "The page title"
author:
  name: "George Washington"
  email: "george.washington@example.com"
---

You can reference page variables like this:

The title of the page is {% raw %}{{ page.title }}{% endraw %}.
```


## Site configuration

The site configuration lives in `_config.yml`. Some variables have special
meaning that tell Federalist how to build your site. All variables can be used
throughout your site like this:

    Contact our agency by email at {% raw %}{{ site.agency_contact.email }}{% endraw %}
    or by phone {% raw %}{{ site.agency_contact.phone }}{% endraw %}.
