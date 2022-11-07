# ocw-hugo-themes

This is a collection of [Hugo](https://gohugo.io/) themes.  They can be used to generate pages related to the OCW website as well as OCW course sites.

## Structure

```
base-theme/
├── assets/
│   ├── css/
│   ├── js/
│   ├── fonts/
│   ├── webpack/ (webpack configuration for building css / js bundles)
│   └── index.ts
├── data/
│   └── webpack.json (describes location of rendered webpack assets)
├── layouts/
│   ├── _default/ (standard Hugo base templates)
│   ├── partials/
│   ├── shortcodes/
│   ├── 404.html
│   └── robots.txt
└── static/
    └── images (images inherited by every theme)
course/
├── assets/
│   ├── css/
│   ├── js/
│   └── course.ts
├── data/
│   ├── departments.json (map of department numbers)
│   └── search_query_keys.json (map of search query strings)
└── layouts/
    ├── _default/
    ├── pages/
    ├── partials/
    ├── resources/
    ├── shortcodes/
    └── home.html
fields/
├── assets/
│   ├── css/
│   ├── js/
│   └── fields.js
└── layouts/
    ├── _default/
    ├── partials/
    └── home.html
www/
├── archetypes/ (various Hugo markdown templates for manually creating content with "hugo new")
├── assets/
│   ├── css/
│   ├── js/ (contains React based search app)
│   └── www.tsx
├── content/
│   └── search/
│       └── _index.md (placeholder to tell Hugo to render the search page)
└── layouts/
    ├── _default/
    ├── instructor/
    ├── pages/
    ├── partials/
    ├── search/
    ├── testimonials/
    └── home.html
package_scripts/ (various scripts for packaging and deployment)
```

## Themes

### base-theme

The base theme should be inherited first whenever using any of the other
themes.  It includes the webpack configuration for building the CSS / JS used
by all the themes as well as the base layouts that include the built assets.
Anything that is to be used by all other themes should be placed here.

### course
![18.06 Linear Algebra Spring 2010](https://user-images.githubusercontent.com/12089658/137996002-ade25b96-9c75-4a43-a3c2-715ed68cc976.png)

The course theme is used to render OCW course sites.  Content is generated by
[`ocw-studio`](https://github.com/mitodl/ocw-studio) and the structure is
defined in
[`ocw-hugo-projects`](https://github.com/mitodl/ocw-hugo-projects/blob/main/ocw-course/ocw-studio.yaml).
The main components are "pages" and "resources."  Course sites can be edited in
an instance of `ocw-studio` and published to a backend like Github as Hugo
markdown content that can be built using the course theme.

### www
![OCW Home Page](https://user-images.githubusercontent.com/12089658/137997162-ab717049-a516-479e-b567-a82a7135e65e.png)

The www theme is primarily responsible for rendering the OCW home page,
although there are a few other types of content it is set up to handle
including instructors that are rendered in a static JSON API, testimonials and
the promotions seen in the carousel.  This content can be edited in an instance
of [`ocw-studio`](https://github.com/mitodl/ocw-studio) and it uses the
`ocw-www` starter configuration in
[`ocw-hugo-projects`](https://github.com/mitodl/ocw-hugo-projects/blob/main/ocw-www/ocw-studio.yaml).

### fields
![Philosphy Fields Page](https://user-images.githubusercontent.com/12089658/166737333-442e2334-6f89-43c9-963c-91e7e0a010aa.png)

The fields theme is used to render collections of course lists,
much like the collections linked from the OCW home page. In this theme,
the field that you specify in your content is used as the home page. This
content can be edited in an instance of [`ocw-studio`](https://github.com/mitodl/ocw-studio)
and it uses the `mit-fields` starter configuration in
[`ocw-hugo-projects`](https://github.com/mitodl/ocw-hugo-projects/blob/main/mit-fields/ocw-studio.yaml).

## Local development

### Dependencies

- [nvm](https://github.com/nvm-sh/nvm#installing-and-updating)
- [NodeJS](https://nodejs.org/en/download/); version managed by nvm
- [Yarn](https://yarnpkg.com/getting-started/install)

If you're running the site for the first time, or if dependencies have changed,
install dependencies with:

`yarn install`

You will also need git access to clone repos from
https://github.mit.edu/ocw-content-rc, so make sure your command line `git`
interface is configured to do so.

The frontend JS code is built using webpack and Typescript. You can run the
Typescript compiler separately by doing

```
yarn typecheck
```

### Obtaining content

Content for the themes in this repo can be generated using an instance of
[`ocw-studio`](https://github.com/mitodl/ocw-studio), a CMS used to author OCW
sites.  The RC instance is located at https://ocw-studio-rc.odl.mit.edu.  Its
content is published to MIT's Github Enterprise instance under the
[`ocw-content-rc`](https://github.mit.edu/ocw-content-rc) organization.  For
the `www` theme, content can be found in the
[`ocw-www`](https://github.mit.edu/ocw-content-rc/ocw-www) repo.  For the
`course` theme, use any repo in the [`ocw-content-rc`](https://github.mit.edu/ocw-content-rc)
organization created using the `ocw-course` starter or create and publish your own.
Much the same for `fields`, you can either create your own site using the
`mit-fields` starter or find an existing one and use that.

### Environment variables

An example environment file can be found at `.env.example`.  To further explain the various environment variables and what they do:

| Variable | Relevant Themes | Example | Description |
| --- | --- | --- | --- |
| `GTM_ACCOUNT_ID` | `base-theme`, `www`, `course` | N/A | A string representing a Google account ID to initialize Google Tag Manager with |
| `SEARCH_API_URL` | `www` | `http://discussions-rc.odl.mit.edu/api/v0/search/` | A URL to an `open-discussions` search API to fetch results from |
| `OCW_STUDIO_BASE_URL` | `www` | `http://ocw-studio-rc.odl.mit.edu/` | A URL of an instance of [`ocw-studio`](https://github.com/mitodl/ocw-studio) to fetch home page content from |
| `STATIC_API_BASE_URL` | `course` | `http://ocw.mit.edu/` | A URL of a deployed Hugo site with a static JSON API to query against |
| `RESOURCE_BASE_URL` | `base-theme` | `https://live-qa.ocw.mit.edu/` | A base URL to prefix the rendered path to resources with |
| `SITEMAP_DOMAIN` | `base-theme` | `ocw.mit.edu` | The domain used when writing fully qualified URLs into the sitemap |
| `WWW_HUGO_CONFIG_PATH` | `www` | `/path/to/ocw-hugo-projects/ocw-www/config.yaml` | A path to the `ocw-www` Hugo configuration file |
| `COURSE_HUGO_CONFIG_PATH` | `course` | `/path/to/ocw-hugo-projects/ocw-course/config.yaml` | A path to the `ocw-course` Hugo configuration file |
| `WWW_CONTENT_PATH` | `www` | `/path/to/ocw-content-rc/ocw-www` | A path to a Hugo site that will be rendered when running `npm run start:www` |
| `COURSE_CONTENT_PATH` | `course` | `/path/to/ocw-content-rc/` | A path to a base folder containing `ocw-course` type Hugo sites |
| `OCW_TEST_COURSE` | `course` | `18.06-spring-2010` | The name of a folder in `COURSE_CONTENT_PATH` containing a Hugo site that will be rendered when running `npm run start:course` |
| `OCW_COURSE_STARTER_SLUG` | `www` | `ocw-course` | When generating "New Courses" cards on the home page, the `ocw-studio` API is queried using `OCW_STUDIO_BASE_URL`.  This value determines the `type` used in the query string against the API |
| `COURSE_BASE_URL` | N/A | `http://localhost:3000/courses` | Used in `build_all_courses.sh`, this is the `--baseUrl` argument passed to each course build iterated by the script |
| `FIELDS_HUGO_CONFIG_PATH` | `fields` | `/path/to/ocw-hugo-projects/mit-fields/config.yaml` | A path to the `mit-fields` Hugo configuration file |
| `FIELDS_CONTENT_PATH` | `fields` | `/path/to/ocw-content-rc/philosophy` | A path to a Hugo site that will be rendered when running `npm run start:fields` |
| `VERBOSE` | N/A | `0` | Used in `build_all_courses.sh`, if set to `1` this will print verbose output from the course builds to the console |
| `DOWNLOAD` | N/A | `1` | Used in `npm run start:course`, if set to `0` this will not download course data from S3 and instead source `OCW_TEST_COURSE` from the specified `OCW_TO_HUGO_OUTPUT_DIR`. If not specified, it will default to 1 and try to download data from S3. |
| `WEBPACK_PORT` | N/A | `3001` | Port used by Webpack Dev Server |
| `WEBPACK_ANALYZE` | N/A | `true` | Used in webpack build. If set to `true`, a dependency analysis of the bundle will be included in the build output. |

### Running ocw-www

To run `ocw-www` locally for working on the `www` theme:

`npm run start:www`

To customize your `www` site:

 - Clone the `ocw-www` content repo: https://github.mit.edu/ocw-content-rc/ocw-www
 - Clone `ocw-hugo-projects` to obtain the relevant configuration file: https://github.com/mitodl/ocw-hugo-projects
 - Set the following environment variables in your `.env` file, replacing `/path/to` with your path to the repos indicated:
   - `WWW_HUGO_CONFIG_PATH=/path/to/ocw-hugo-projects/ocw-www/config.yaml`
   - `WWW_CONTENT_PATH=/path/to/ocw-content-rc/ocw-www/`
 - Optionally set these environment variables as well, depending on the functionality you need to work on:
   - `SEARCH_API_URL=https://discussions-rc.odl.mit.edu/api/v0/search/` (for testing search functionality)
   - `OCW_STUDIO_BASE_URL=http://ocw-studio-rc.odl.mit.edu/` (for testing `ocw-studio` API functionality, such as the "new courses" section rendered by `www/layouts/partials/home_course_cards.html`)
   - `RESOURCE_BASE_URL=https://live-qa.ocw.mit.edu/` (if you need to test the loading of resources from S3 or some other CDN)
 - Start the site with `npm run start:www`
 - The site should be available at http://localhost:3000/

### Running a course site

To run a course site locally for working on the `course` theme:

`npm run start:course`

To customize your `course` site:

 - Create a course site at https://ocw-studio-rc.odl.mit.edu/sites/ using the `ocw-course` starter
 - Add content and resources to your site relevant to what you're working on and publish your site to staging. Ensure course metadata has been created.
 - Find your site at https://github.mit.edu/ocw-content-rc/ and clone it locally (it's recommended to have a parent folder locally for all cloned courses, in this case `ocw-content-rc`)
 - Clone `ocw-hugo-projects` to obtain the relevant configuration file: https://github.com/mitodl/ocw-hugo-projects
 - Set the following environment variables in your `.env` file, replacing `/path/to` with your path to the repos indicated and `your-course-slug` with the folder your course was cloned into in a previous step:
   - `COURSE_HUGO_CONFIG_PATH=/path/to/ocw-hugo-projects/ocw-course/config.yaml`
   - `COURSE_CONTENT_PATH=/path/to/ocw-content-rc/`
   - `OCW_TEST_COURSE=your-course-slug`
 - Optionally set these environment variables as well, depending on the functionality you need to work on:
   - `RESOURCE_BASE_URL=https://live-qa.ocw.mit.edu/` (if you need to test the loading of resources from S3 or some other CDN)
   - `STATIC_API_BASE_URL=https://live-qa.ocw.mit.edu/` (for loading content from a static API like the instructors published by `ocw-www`)
 - Start the site with `npm run start:course`
 - The site should be available at http://localhost:3000/

### Running a fields site

To run a course site locally for working on the `fields` theme:

`npm run start:fields`

To customize your `fields` site:

 - Create a course site at https://ocw-studio-rc.odl.mit.edu/sites/ using the `mit-fields` starter
 - Visit the "Subfields" section and define as many subfields as you want, adding a list of courses to each one
 - Visit the "Field" section and fill out the info about the field site you are creating, selecting a featured Subfield and adding as many other subfields as you want
 - When you are done editing, publish your site
 - Find your site at https://github.mit.edu/ocw-content-rc/ and clone it locally (it's recommended to have a parent folder locally for all cloned courses, in this case `ocw-content-rc`)
 - Clone `ocw-hugo-projects` to obtain the relevant configuration file: https://github.com/mitodl/ocw-hugo-projects
 - Set the following environment variables in your `.env` file, replacing `/path/to` with your path to the repos indicated and `your-field-slug` with the folder your course was cloned into in a previous step:
   - `FIELDS_HUGO_CONFIG_PATH=/path/to/ocw-hugo-projects/mit-fields/config.yaml`
   - `FIELDS_CONTENT_PATH=/path/to/ocw-content-rc/your-field-slug`
   - `STATIC_API_BASE_URL=https://live-qa.ocw.mit.edu/` (for loading content from a static API for the courses linked in your Subfields)
 - Optionally set these environment variables as well, depending on the functionality you need to work on:
   - `RESOURCE_BASE_URL=https://live-qa.ocw.mit.edu/` (if you need to test the loading of resources from S3 or some other CDN)
 - Start the site with `npm run start:fields`
 - The site should be available at http://localhost:3000/

### Writing Tests
Most tests in OCW Hugo Themes should be written as e2e tests with Playwright. See [End to End Testing](./tests-e2e/README.md) for more.

### Miscellaneous commands

- `WEBPACK_ANALYZE=true yarn run build:webpack`: This builds the project for production and should open a an analysis of the bundle in your web browser. 

### External API's

The `www` theme accesses external API's made available by
[`ocw-studio`](https://github.com/mitodl/ocw-studio) and
[`open-discussions`](https://github.com/mitodl/open-discussions) for some
functionality.  Search results are provided by `open-discusisons` and
`ocw-studio` provides some content for the home page, such as newly added
courses and news items.  If you need to work with this functionality you can
either run a local instance of either of these projects, or alternatively point
at the RC instances and temporarily disable CORS in your browser.

### CORS

The search page at `/search` uses the `open-discussions` search API to source
results.  Running this locally and populating it with results can be tedious,
so it's often easier to just point your local website at an already running
version of the search API.  In order for this to work properly, you will need
to disable CORS.  This is a generally unsafe thing to do and you should make
sure that in whatever browser you open with CORS disabled, you are only testing
your local `ocw-www` site and not visiting other sites.  Here is a link that
shows how to do this in various browsers:
https://medium.com/swlh/avoiding-cors-errors-on-localhost-in-2020-5a656ed8cefa
