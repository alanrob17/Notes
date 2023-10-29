## Using Hugo to Create a Website

### Important note

I upgraded my version of Hugo and all my sites stopped working. I tried reinstalling with Chocolatey but unfortunately it only installs the latest version of Hugo.

I uninstalled Hugo and then used **npm** to globally install my version of Hugo.

```bash
    npm install -g hugo-extended@0.91.2
```

Thankfully this fixed my problem and I am able to run Hugo locally on my PC again.

**Note:** ignore the instructions below on installing Hugo with Chocolatey.

### My Portfolio blog

#### alanrobson.co

```bash
    git remote add origin https://github.com/alanrob17/Portfolio.git
```

#### alanrobson.au

```bash
    git remote add origin https://github.com/alanrob17/technical-blog.git
```

### Installing Hugo

```bash
    choco install hugo -confirm

    choco upgrade hugo -confirm --force

    hugo new site mysite

    cd mysite
```

### Installing Hugo Extended

```bash
    choco​ install -y hugo-extended
```

### Running Hugo locally

```bash
    hugo server
```

Or to stop having to restart (compiles slower).

```bash
    hugo server --disableFastRender
```

> Ctrl-c to shutdown.

### Installing a theme

Go to the themes folder

cd themes

```bash
    git clone https://github.com/eueung/hugo-casper-two.git casper-two
```

This will install the Casper Two theme.

I use this theme for ``alanrobson.co``.

### How to implement the theme

Go back to mysite

```bash
    cd ..
```

now we are going to edit the config file in VS Code

```bash
    code .
```

Edit config.toml

Grab the example config.toml from the Casper Two web page and overwrite what is already in the page.

### Adding Content

Create a new post named javascript training.

```bash
    hugo new post/javascript-training.md
```

### Creating a page

```bash
hugo new privacy.md
```

### Dumping your content

This makes a quick HTML copy of your site.

```bash
    hugo --theme=casper-two --buildDrafts
```

This dumps your content into a folder named **public**.

### Hugo Themes

#### Ace

[Ace blog theme.](https://themes.gohugo.io//theme/ace-documentation/getting-started/ "Ace blog theme.")

#### Mainroad

This is the theme I am using for [alanrobson.au](https://alanrobson.au "My technical blog").

[Mainroad blog theme.](https://themes.gohugo.io/mainroad/ "Mainroad blog theme.")

#### Casper Two

This is the theme I am using for [alanrobson.co](https://alanrobson.co "My blog").

[Casper Two theme.](https://github.com/eueung/hugo-casper-two "Casper Two theme.")

### Hugo information

Tags and Categories.

[Hugo tags and categories.](https://sam.hooke.me/note/2018/03/hugo-tag-and-category-pages/ "Casper Two theme.")

Adding a form to Netlify.

[Add a form.](https://blog.chrisstayte.com/coding/buildawebsitefor12dollars/2018-add-a-contact-to-your-netlify-powered-website/ "Add a form.")

[Partial web pages.](https://discourse.gohugo.io/t/creating-static-content-that-uses-partials/265/15 "Add a form.")

### Adding syntax highlighting

```bash
    {{< highlight go "linenos=inline,linenostart=1" >}}
        s = remove from all sub folders.
        w = write changes to all files with that particular phrase in it.
            if you ignore this it will just produce a log file with all of the changes to take place.
```

Or

```bash
    {{< highlight go "linenos=table,hl_lines=8 15-17,linenostart=199" >}}
    // ... code
    {{< / highlight >}}
```

----

## My Hugo websites

I have two live Hugo websites.

[alanrobson.co](https://alanrobson.co "My blog")

[alanrobson.au](https://alanrobson.au "My technical blog")

Note that both of these sites have the same content but have different themes.

### Updating my Hugo Websites

#### alanrobson.co

open the site ``projects\alanrobson.co`` in Visual Studio Code.

##### Running Hugo locally

```bash
    hugo server --disableFastRender
```

##### Adding a new post

Stop the **hugo server**. Then run.

```bash
    hugo new post/the-name-of-my-new-post.md
```

This creates a new blog post in your blog.

##### Editing a post

You edit content in the ``content\post`` folder.

You add new images to the ``static\images`` folder.

If you have added a new post into the blog you must change the header in the post.

The following is a sample header from one of my posts in the **Casper-Two** format.

```markdown
    ---
    title: "Changing Link Program to Web Request"
    date: 2019-11-21T13:46:20+10:00
    draft: false
    image: "images/link.jpg"
    tags: ["csharp"]
    categories: ["programming"]
    ---
```

**Where:**

* **title:** is the name of the post and this appears at the top of your page.
* **date:** is the date the page was generated. You can manually change this.
* **draft:** making this ``false`` means publish (make it visible) it on your blog.
* **image:** is the background image that appears at the top of the page. These are approximately 1280*950 pixels in size and I will download landscape images from [Unsplash.com](https://unsplash.com "Unsplash.com") in medium size.
* **categories** these are seen of the front page and the blog page as a menu. At present I only have two categories and they are **programming** and **music**.
* **tags:** I can create any tag that refers to the category that I am under. For example, my *category* is *programming* and my tag is *csharp*. You can have multiple tags for each post.

You can add images to the post and they have to be in this format, ``../images/image.jpg``.

##### Saving your files to Git

Once you have finished editing, commit and push your files to Github.

Github triggers a process on [Netlify.com](Netlify.com "Netlify.com") that builds the content and deploys it to [alanrobson.co](https://alanrobson.co "My blog").

#### alanrobson.au

open the site ``projects\alanrobson.au`` in Visual Studio Code.

##### Running Hugo locally

```bash
    hugo server --disableFastRender
```

##### Adding a new post

Stop the **hugo server**. Then run.

```bash
    hugo new post/the-name-of-my-new-post.md
```

This creates a new blog post in your blog.

##### Editing a post

You edit content in the ``content\post`` folder.

You add new images to the ``static\images`` folder.

If you have added a new post into the blog you must change the header in the post.

The following is a sample header from one of my posts in the **Mainroad** format.

```markdown
    ---
    title: "Changing Link Program to Web Request"
    description: Using Web Request to automatically download web pages.
    date: 2019-11-21T13:46:20+10:00
    draft: false
    tags: 
        - "CSharp"
    categories: 
        - "Programming"
    ---
```

You can add images to the post and they have to be in this format, ``/images/image.jpg``.

**Note:** the image text format is different to the *Casper-Two* format.

##### Saving your files to Git

Once you have finished editing, commit and push your files to Github.

Github triggers a process on [Netlify.com](Netlify.com "Netlify.com") that builds the content and deploys it to [alanrobson.au](https://alanrobson.au "My technical blog").
