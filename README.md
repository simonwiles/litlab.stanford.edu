# LitLab.stanford.edu guide

## How the site works

In the big picture, all the site content -- project descriptions, Techne posts, pamphlets, information about Lab members -- exists as a series of text files, mostly using [markdown](https://www.markdownguide.org/basic-syntax/) syntax, which get processed through a set of templates to create the website. To update the site contents, you create new markdown files or update existing ones in the *src* directory; you can also add images or other files and reference them in your Markdown files.

The one exception is the information about Lab members, which is managed in a JSON file at *src/data/LabPeople.json*.

We'll go through the site one content type at a time.


## Content

### Techne

Techne posts live in *src/techne*. Their filenames should follow the following convention: `yyyy-mm-dd-post-title-separated-by-hyphens.md`. At the top of each Techne post markdown file, there's *frontmatter* -- basically, metadata. The frontmatter section begins and ends with three hyphens, and you should fill in the values for each of the fields. Here's an example:

    ---
	permalink: '/techne/example-post/'
    title: "Techne's Example Post"
    authors: [efredner, malgeehewitt]
    projects: [typicality]
    date: 2020-07-18
    teaser: |
      A sentence or two goes here, and you don't have to worry about single or double quotes.
	---

Text that isn't part of a list should be surrounded by either single or double quotes; if the text includes an apostrophe (like in the example title here), use double quotes, and vice-versa.

Lists should be in square brackets, and you don't have to put quotes around list items. The two lists associated with Techne posts are *authors* and *projects*. To reference a Lab member or collaborator, use the first letter of their first name and their full last name, all lower-case. (If you're not sure look at the *src/data/LabPeople.json* file, which is ordered alphabetically by last name.)

To reference one or more projects, use the project key (unique identifier). The key is the final part of the project URL, and is also the name of the project's markdown file in the *src/projects* directory.

### Presentations
Presentation posts are like Techne, but simplified. Presentation posts should follow the same naming convention: `yyyy-mm-dd-post-title.md`.

Here's the frontmatter for a presentation post:

    ---
    title: "LitLab at MLA 2023"
    projects: [typicality, star-wars]
    date: 2023-01-01
    teaser: |
      The LitLab is doing stuff at MLA 2023.
    ---

In the body of the presentation post, you can put the full description of Lab talks.

### Projects & Pamphlets
All projects live in *src/projects*, and pamphlets are generated by metadata in the frontmatter of projects with an associated pamphlet. Projects that should no longer be on the Lab's list of current projects are assigned `status: archive`, which will put them on an archive page.

Project pages can have extensive prose in the content area below the frontmatter, or just a little metadata in the frontmatter. At a minimum, each project should have a title, a key, information about the project team, and one square image (400px x 400px square, to be put in *src/assets/images*). 

If the project has the pamphlet, all the metadata in the pamphlet section should be filled out. Pamphlet PDFs should go in *src/assets/pdf*.

Here's an example of frontmatter for an archived project with a pamphlet:

    ---
    key: 'example-with-pamphlet'
    permalink: /projects/example-with-pamphlet/
    title: "Example Project with a Pamphlet"
    image: '/assets/images/example-with-pamphlet.jpg'
    members: [malgeehe, adroge, efredner, rheuser, amanshel, nnomura]
	collaborators: [jporter, hwalser]
	date_updated: 2023-01-01
	start_date: 2018-09-01
	end_date: 2023-01-01
    status: 'archive'
    shortdesc: "Example with pamphlet short description here."
    longdesc: "Example with pamphlet long description here. Note how it is longer than the short description."
    pamphlet:
      p_title: "Pamphlet Example"
      p_image: "/assets/images/p99.png"
      p_pdf: "/assets/pdf/LiteraryLabPamphlet99.pdf"
      p_pubdate: 2019-09-01
      p_prose: "This pamphlet explores the phenomenon of examples in literary history."
    ---

### People
All the information about Lab members and collaborators is in a JSON file: *src/data/LabPeople.json*. The entries are arranged in alphabetical order by last name. Every person has a unique key: the first letter of their first name, and then their full last name, all lower-case, single-word, no punctuation. So, the key for Mark Algee-Hewitt is `malgeehewitt`.

Members of the Core Research Team and other people with specific titles (i.e. "Director" variants) have a title field and a "bio" field. Active Stanford-based members have the value "stanford" in their status field; active members outside of Stanford have the value "elsewhere" in their status field, and also have a "university" field with their current location (if relevant). Current members at Stanford or elsewhere have an "email" field and an optional "website" field.

If you want to reference a person for the first time (e.g. as part of a project team or as an author of a Techne post), you have to add them to the LabPeople.json file and then reference their key in the post or project.


## Layout

### Pages in the navigation menu
Other than the index page, which appears directly within the *src* folder, the main pages of the site live in *src/pages*. Pages appear in the navigation menu because of something added to the frontmatter. For instance, this is the frontmatter for the About page:

    ---
	permalink: /about/
	title: 'About'
	layout: base
	eleventyNavigation:
	  key: About
	  order: 1
	---

The permalink indicates its path. The layout for all the main pages is 'base' (i.e. just the plain page layout). The *eleventyNavigation* section is what indicates that the page belongs in the menu, and in what order it will appear.

With the exception of the About page, each of the navigation menu pages includes some combination of text and code to pull in data from somewhere else (e.g. a list of Techne posts and teasers, a grid of Projects, etc.) These layouts use [Nunjucks](https://mozilla.github.io/nunjucks/templating.html#tags) (.njk) which is fairly similar to Liquid (the language used for templates in Jekyll).

### Projects
The templating / layout for all the project pages is controlled by *src/layouts/project.njk* (**not to be confused** with *src/pages/projects.njk*, which contains the framing text and the layout for the Projects page in the navigation menu.) Edit that file to update the layout for all projects.

### Posts
The templating / layout for both Techne and Presentations is handled by *src/layouts/post.njk*. 