# paulapivat

Website and Portfolio

# Quick Start
- navigate to correct directory (/RCode/paulapivat)
- blogdown::serve_site()

# Setup

Alison Hill, ['Up & Running with blogdown'](https://alison.rbind.io/post/2017-06-12-up-and-running-with-blogdown/)

# Netlify Deployment

[Resource](https://gohugo.io/hosting-and-deployment/hosting-on-netlify/)

see netlify.toml
add HUGO_VERSION = 'x.xx.x'

# Customizing Acadmic Theme

[Academic Theme Documentation](https://sourcethemes.com/academic/docs/)

## Theme Color

/* Color Theme Swatches in Hex */
.Alizarin-crimson-1-hex { color: #171717; }
.Alizarin-crimson-2-hex { color: #631111; }
.Alizarin-crimson-3-hex { color: #E32626; }
.Alizarin-crimson-4-hex { color: #8F7474; }
.Alizarin-crimson-5-hex { color: #F5F5F5; }

/* Color Theme Swatches in RGBA */
.Alizarin-crimson-1-rgba { color: rgba(22, 22, 22, 1); }
.Alizarin-crimson-2-rgba { color: rgba(99, 16, 16, 1); }
.Alizarin-crimson-3-rgba { color: rgba(226, 38, 38, 1); }
.Alizarin-crimson-4-rgba { color: rgba(142, 115, 115, 1); }
.Alizarin-crimson-5-rgba { color: rgba(244, 244, 244, 1); }

/* Color Theme Swatches in HSLA */
.Alizarin-crimson-1-hsla { color: hsla(0, 0, 9, 1); }
.Alizarin-crimson-2-hsla { color: hsla(0, 71, 22, 1); }
.Alizarin-crimson-3-hsla { color: hsla(0, 77, 51, 1); }
.Alizarin-crimson-4-hsla { color: hsla(0, 10, 50, 1); }
.Alizarin-crimson-5-hsla { color: hsla(0, 0, 96, 1); }

## Fonts

Google Font: family=Domine:wght@400;700

# Homepage Widgets

## Inactive
active = false
- hero
- featured
- publications
- experience
- talks

## Active (weights, ordering)
active = true
- demo (15)
- about (20)
- projects (50)
- posts (60)
- accomplishment (65)
- gallery (66)
- tags (120)
- contact (130)

# Menu (weights)

- Start Here ('#', 10)
- About ('#about', 15)
- Projects ('#projects', 20)
- Posts ('#posts', 30)
- News ('#accomplishments', 40)
- Viz ('#gallery', 45)
- Courses ('courses/', 50)
- Contact ('#contact', 60)

# Edit Start Here section
- path: paulapivat/content/home/demo.md

# Edit About Section
- path: paulapivat/content/authors/admin/_index.md

# Edit Menu
- path: paulapivat/config/_default/menus.toml

# Add Project(s)

- Files: paulapivat/content/project/
- Add new folder
- Inside new folder, add index.md
- Copy TOML structure from dummy projects
- To add a main picture for each project page, save jpeg as `featured.jpeg` and drop it into the project folder.

## Add PDF slides for each Project 
- follow this [guide](https://www.themillerlab.io/post/pdf-to-jpeg/)

- Navigate to Folder: content/slides
- In the *slides* folder create a project-specific-folder (i.e., wpa)
- In that folder, create index.md (which controls content of slides)
- Convert PDF to jpeg using `pdftools` package
- pdf_convert("your_pdf.pdf", format = "jpeg", pages = NULL, filenames = NULL, dpi = 300, opw = "", upw = "", verbose = TRUE)
- dpi is jpeg resolution
- Fill in index.md with slides (see [guide](https://www.themillerlab.io/post/pdf-to-jpeg/))

- Return to paulapivat/content/project/project-folder/index.md to change `slides` value in TOML
- NOTE: both files are in the `content` folder, one in `slides` directory another in `project` directory they talk to each other although they reside in different directories.

## Add Project Filter Button / Tag on Front Page
- Edit: paulapivat/content/home/projects.md
- Note must change tags section on each individual project to match

## Add Blog Posts (Rmd file, without own folder)
- path: /content/post/
- Rmd file
- NB: file name yyyy-mm-dd-slug **note** dates must be current or else it won't render 
- NB: within YAML settings, make sure `slug` is unique or else it will over write another blog post


## Add Blog Posts (with own folder)

## Edit Blog Section
- path: paulapivat/content/home/posts.md
- add external link to [Getwyze.com](http://getwyze.com/)
- edit how many blog posts to display (currently 2)

## Edit Accomplishments section
- path: paulapivat/content/home/accomplishments.md

## Edit Gallery
- path: paulapivat/content/home/gallery/gallery

## Add Technical Notes 
- add in navigation menu: config/_default/menus.toml
- add [[main]], name, url, weight (below Courses tab)
- add content/technical_notes (match technical_notes/ url); same level as courses

### Separate Technical Notes from Courses

- add content/technical_notes/example_tech (folder; distinct from content/courses/example)

- path content/technical_notes/example_tech/_index.md 
- change yaml: menu: example_tech: (previously *just* example)

- add _index.md, *outside* example_tech folder (path: content/technical_notes/)
- change yaml title: technical_notes

- add _index.md, *inside* example_tech folder (path: content/technical_notes/example/)
- change yaml title: Technical Notes Overview
- change yaml menu: example_tech: (previously *just* example)

- path content/technical_notes/example_tech/technical_notes1.md
- change yaml menu: example_tech: (note: links to _index.md which also has yaml menu: example_tech: )

- path content/technical_notes/example_tech/technical_notes2.md
- change yaml menu: example_tech: (note: links to _index.md which also has yaml menu: example_tech: )

### Adding a new technical note

- path: content/technical_notes/example_tech/file_name_of_new_technical_note.md
- pay attention to actual file name (e.g., file_name_of_new_technical_note.md)
- change yaml linktitle: (will appear on side menu)
- change yaml menu: parent: Topic Heading (will appear on side menu)
