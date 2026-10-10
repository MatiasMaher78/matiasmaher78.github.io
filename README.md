# matiasmaher78.github.io

Source for the portfolio of **Matias Maher**, Data Engineer: [matiasmaher78.github.io](https://matiasmaher78.github.io)

The site presents project write-ups in data engineering, ETL and automation, data quality and BI, and computer vision dataset engineering.

## Content

| Project | Type | Context |
|---|---|---|
| Autoparts Integrator | Computer vision · human-in-the-loop · dataset engineering | Independent product, in active development |
| Vehicle Data Print | Full-stack homologation tool | Independent product, validated with real users |
| Automotive OEM Pricing ETL | ETL · web scraping | Reciclauto |
| Automotive OEM Validation | Browser automation · data quality | Reciclauto |
| Historical Sales & Data Quality Dashboard | Power BI · DAX | Reciclauto, demo data |

Project pages live in `_posts/`, the homepage in `index.html` and the About page in `aboutme.md`. Projects are grouped on the homepage by the `project_type` front matter field (`cv`, `mvp`, `automation`, `bi`).

Business figures in the dashboard write-up are transformed demo values. Project source code and credentials are not part of this repository; sanitized portfolio versions of some projects are published as separate repositories.

## Local build

Requires Ruby and Bundler.

```bash
bundle install
bundle exec jekyll serve
```

## Theme and license

Built with [Beautiful Jekyll](https://github.com/daattali/beautiful-jekyll) by [Dean Attali](https://deanattali.com), released under the MIT License (see [`LICENSE`](LICENSE)). Theme documentation is available at [beautifuljekyll.com](https://beautifuljekyll.com); the theme's changelog is kept in [`CHANGELOG.md`](CHANGELOG.md).
