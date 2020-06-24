# paulapivat

Website and Portfolio

# Quick Start
- navigate to correct directory (/RCode/paulapivat)
- blogdown::serve_site()

# Setup

Alison Hill, 'Up & Running with blogdown'[https://alison.rbind.io/post/2017-06-12-up-and-running-with-blogdown/]

# Netlify Deployment

Resource: https://gohugo.io/hosting-and-deployment/hosting-on-netlify/

see netlify.toml
add HUGO_VERSION = 'x.xx.x'

# Customizing Acadmic Theme

Academic Theme Documentation: https://sourcethemes.com/academic/docs/

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


