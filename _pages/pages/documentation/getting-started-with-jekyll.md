---
title: Getting Started With Jekyll
permalink: /pages/documentation/getting-started-with-jekyll/
layout: docs
navigation: pages
sidenav: pages-documentation
redirect_from: 
  - /pages/using-federalist/getting-started-with-jekyll/

---

_Welcome! The intended audience of this guide is content creators who have
limited technical knowledge and no HTML, CSS, or YAML experience. Keep in mind
that each template and theme is a little different. If you get stuck, don't
hesitate to [reach out](mailto:{{ site.support_email }})._

Federalist monitors your site’s GitHub repository (or repo) for changes you
make. It will automatically build a new version of your site in a few minutes.
You can view the live site [from Federalist][federalist-sites] and clicking
"View site".

{% capture content %}
If you’re unfamiliar with GitHub, the Federalist team recommends that you
[familiarize
yourself](https://federalist.18f.gov/documentation/instructional-demos/#introduction-to-github-for-newcomers)
before continuing through this guide.
{% endcapture %}
{% include pages/components/alert--note.html content=content %}


## Getting started with Jekyll

Immediately after building a new site, you need to customize the general content
on your site, including your site title and office’s contact information. To do
that, you’ll need to edit the configuration options in `_config.yml`. Keep in
mind that this file is a special file type called “YAML” that requires specific
formatting. Indentation is important to keep related options together — in order
for Jekyll to successfully read the configuration file so that the site will
build with the proper settings, the correct indentation must be used. We
recommend using space characters instead of tabs for nesting configuration
options. Learn more about [working with YAML][resources-yaml].

Here's an example `_config.yml` snippet:

```yaml
# This is a YAML comment. A comment starts
# with the # character. Comments are
# ignored by Federalist.
title: This is the site title

theme: uswds-jekyll

site_width: wide

# GitHub information
# This is used for adding an edit this page link
# to the footer
github:
  organization: 18F
  repository: federalist-uswds-template
  default_branch: report-config
# Notice how two space characters are used to
# indent the properties. This means they are
# all children of the “github” property.
```

Your content lives in the pages/ directory. Click the pages/ directory in
GitHub to see your existing pages. Select “page.md” from the list of options.
Once you’re on page.md, look for the pencil icon near the top right of the
content window to edit the page.

![Screenshot showing a site page on GitHub and the edit page pencil icon]({{ site.baseurl }}/assets/images/getting-started--edit-page-pencil-icon.png)

The three dashes `---` separate the “front matter” of the page from the content
of the page. Front matter is used to instruct the build system about how to
build the page and provide additional data. Learn more about
[front matter][resources-front-matter].

```yaml
---
# This is front matter.
title: Title of your page
---

Content for your page goes here.
```

The content of your page starts below the second set of three dashes `---` that
end the front matter section. The content is all in markdown format,
a plain-text simple [markup language](https://en.wikipedia.org/wiki/Markup_language)
designed to be readable for content editors. Try editing your heading by
replacing the text “Welcome to the Federalist Report Template” with “Hello
world!”. Learn more about [Markdown][resources-markdown].

Once you’re finished making content changes, scroll to the bottom of the page
and click “Commit changes.” Federalist will automatically detect your changes
and start building your site. It should be live within a few minutes.

{% capture content %}
When making a commit, it’s best to describe the change in
GitHub’s commit summary, along with why you’re making the change in the commit
description. This helps members of your team understand the motivation behind
the changes you’ve made when looking back on the repo’s history.
{% endcapture %}
{% include pages/components/alert--best-practice.html content=content %}

Once your change is committed, Federalist will start rebuilding your site. You
can see the build progress on the [build history page][federalist-sites].<!--
TODO link to this site’s build history page. -->


## Agency logo and other assets

Images, PDFs, and other static files served with your site are called “assets”.
We recommend they be stored in the assets/ directory.

{% capture content %}
There’s no standard practice, but many projects group similar assets into
directories like `assets/images` for images or `assets/css` for CSS files.
{% endcapture %}
{% include pages/components/alert--tip.html content=content %}

Go to the assets/ directory on GitHub. Then click “Upload files”. This will
allow you to upload files like your agency logo to your repo. You’ll then be
able to reference the asset by using a path like `/assets/agency-logo.png` in
your content and in `_config.yml`.


## Create a new page

Click the pages/ directory in GitHub, then click the “Create new file” button.
Give the file a name with an `.md` extension.

{% capture content %}
The filename should be relatively short and use only lowercase
letters, numbers, dashes, and underscores. Federalist will use this filename in
the page’s URL by default, so keeping your URLs short and consistent will make
them easier to read and remember. `open-source-policy.md` is a great filename
for a page containing your agency’s open source policy.
{% endcapture %}
{% include pages/components/alert--best-practice.html content=content %}

You’ll want to include some basic front matter. Copy and paste this example to
start:

```markdown
---
title: "Your new content page"
layout: docs
permalink: /link/to/your-new-content-page/
---

## Your new content page heading

And your content goes here.
```

`title` is the title of your page. `layout` specifies which layout to use. This
is usually `page`, meaning that your content is going to be put onto the default
“page” HTML layout. `permalink` is the URL to your new page. Don’t forget to
commit your changes when you’re done.


## Template-specific customization

You’ll want to refer to the [template
specific documentation]({{ site.baseurl }}/documentation/templates/)
to learn how to configure your template beyond what is covered in this guide.

Once you've got the basics down, we recommend that you check out our [customization
guide]({{ site.baseurl }}{% link _pages/pages/documentation/customization.md %}) for
some more details on how to customize your site.


[Federalist]: {{ site.baseurl }}
[federalist-sites]: {{ site.baseurl }}/sites
[resources-front-matter]: {{ site.baseurl }}{% link _pages/pages/documentation/resources.md %}#working-with-front-matter
[resources-markdown]: {{ site.baseurl }}{% link _pages/pages/documentation/resources.md %}#working-with-markdown
[resources-yaml]: {{ site.baseurl }}{% link _pages/pages/documentation/resources.md %}#working-with-yaml
