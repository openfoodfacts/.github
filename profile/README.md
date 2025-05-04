<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://static.openfoodfacts.org/images/logos/off-logo-horizontal-dark.png?refresh_github_cache=1">
  <source media="(prefers-color-scheme: light)" srcset="https://static.openfoodfacts.org/images/logos/off-logo-horizontal-light.png?refresh_github_cache=1">
  <img height="48" src="https://static.openfoodfacts.org/images/logos/off-logo-horizontal-light.svg">
</picture>

<img id="major_components_map" width="963" alt="image" src="https://github.com/user-attachments/assets/57bcc41c-78ea-47ec-aa15-3784f1c48210">


# Open Food Facts

[![Project Status](http://opensource.box.com/badges/active.svg)](http://opensource.box.com/badges)
[![Crowdin](https://d322cqt584bo4o.cloudfront.net/openfoodfacts/localized.svg)](https://translate.openfoodfacts.org/)
[![Open Source Helpers](https://www.codetriage.com/openfoodfacts/openfoodfacts-server/badges/users.svg)](https://www.codetriage.com/openfoodfacts/openfoodfacts-server)
[![Backers on Open Collective](https://opencollective.com/openfoodfacts-server/backers/badge.svg)](#backers)
[![Sponsors on Open Collective](https://opencollective.com/openfoodfacts-server/sponsors/badge.svg)](#sponsors)


## What is 🍊 Open Food Facts?

### A food products database

Open Food Facts is a database of food products with ingredients, allergens, nutrition facts and all the tidbits of information we can find on product labels.

### Made by everyone

Open Food Facts is a non-profit association of volunteers.
25.000+ contributors like you have added 4 million products from 150 countries using our cross-platform Flutter app for Android & iPhone or their camera to scan barcodes and upload pictures of products and their labels.

### For everyone

Data about food is of public interest and has to be open. The complete database is published as open data and can be reused by anyone and for any use. Check-out the cool reuses or make your own!

* <https://world.openfoodfacts.org/discover>

## How can I help?

To start contributing, the easiest way it to join us on Slack <https://slack.openfoodfacts.org/> and post an introduction about, what you're interested in, and what you would like to do.
This would allow other contributors to pinpoint projects that could match your interests. For example:

> I'm interested by the environemental impact of food and would like to help. I'm a designer, but I would also be interested by how you estimate the environmental impact of products.

> I'm using the App, and wondering how could I help to improve it.

> I'm a student, in computer science, and would like to help with some development. I already do some React, and Python.

### Main roles

Since we are on GitHub, you can guess that Open Food Facts needs some contributions from developers and designers (main projects are detailed in next section). But there are plenty of other ways to contribute. For example, you could:

- Add and clean data: You have a product at home, take time to scan it to see if the data is up to date.
- Spread the word: Speak about the project around you to grow up the community. We [have documents to help you](https://blog.openfoodfacts.org/en/news/presenting-open-food-facts-at-events-study-case).
- Translate the project: to be more accessible, the [pages needs to be translated](https://crowdin.com/project/openfoodfacts).
- Improve food understanding: You're interested by the meaning of labels, by distinctions between the various kinds of tomato sauces, we need your help to improve how data are structured.
- You want to reuse the data for creating your own application, of some scientific studies, feel free to contact us to present the project and ask for help/explanations about data
- Any other improvement you can think about

## 🐛 Reporting problems or asking for a feature

Have a bug or a feature request? Please search for existing and closed issues. If your problem or idea is not addressed yet, please open a new issue. You can ask directly in the discussion room if you're not sure

## 🌐 Translate Open Food Facts in your language

You can help translate the Open Food Facts web version and the app at :
<https://translate.openfoodfacts.org/> (no technical knowledge required, takes a minute to signup)

## 👩‍💻 Developers

Here are the main development projects, under active development:

- Open Food Facts servers (Perl | HTML/CSS | JS)

  This repository is the main website (openfoodfacts.org) and the API used by other applications.

  Due to the implementation of the new design, there is a bunch of small CSS issues to be fixed, and some UX improvements.

  [The repository](https://github.com/openfoodfacts/openfoodfacts-server) | [What can I work on?](https://github.com/openfoodfacts/openfoodfacts-server/issues/5529)
  
- Mobile app (Flutter | Dart)

  This is the official mobile application, a very important tool that help people in their everyday choices about food and also invite them to contribute to the database.
  
  [The repository](https://github.com/openfoodfacts/smooth-app/) | [What can I work on?](https://github.com/openfoodfacts/smooth-app/issues/525)
  
  Its companion project is the [dart-sdk](https://github.com/openfoodfacts/openfoodfacts-dart/)

- Taxonomy editor (Python/React)

  An application made with Python/React that simplifies manipulation of the taxonomy (the Knowledge Graph explaining that - for example - the yogurt is a kind of milk food).
  
  This project has the advantage of being well scoped, and new (development started in 2022). The disadvantage being the complexity of the taxonomy which can take some time to fully understand.

  [The repository](https://github.com/openfoodfacts/taxonomy-editor) | [What can I work on?]()
  
- Robotoff (Python)
  
  This project groups the machine learning pipelines used by Open Food Facts to simplify contribution. Detecting labels, extracting ingredients…

  Most of the code is written in Python, and there's a need for both improving machine learning methods, but also improving data management and API interface.

  A lot of experiments have already been done. Some of them failed, others need refinement. Better ask before starting an issue to avoid rabbit holes 🐰😉
  
  [The repository](https://github.com/openfoodfacts/robotoff) | [What can I work on?](https://github.com/openfoodfacts/robotoff/issues/374)
  
  ML research and models can be found in [openfoodfacts-ai repository](https://github.com/openfoodfacts/openfoodfacts-ai/)

- Hunger Games (React)

  A web app used to gamify contribution. It's a React web app that asks questions based on predictions made by Robotoff.

  [The repository](https://github.com/openfoodfacts/hunger-games/) | [What can I work on?](https://github.com/openfoodfacts/hunger-games/issues/37)
  
- Other currently important projects:
  - [Open Food Facts Explorer](https://github.com/openfoodfacts/openfoodfacts-explorer) - A new experimental web frontend for Open Food Facts, written in Svelte
  - [Open Food Facts Events](https://github.com/openfoodfacts/openfoodfacts-events) could be the bare bone of gamification, user dashboard, data moderation, but needs help to fix some bug and extend it
  - [Folksonomy engine](https://github.com/openfoodfacts/folksonomy_api) can help extend product data with with free attributes (especially useful for Open Products Facts). Bug fixes are needed as well as some extensions

### 🎨 Design
* [Design team](https://github.com/openfoodfacts/openfoodfacts-design) ([What can I work on ?](https://github.com/openfoodfacts/openfoodfacts-design/issues/3)) ([UXR & Design project management accross Open Food Facts](https://github.com/orgs/openfoodfacts/projects/11))

### Frontend
* [Open Food Facts Explorer](https://github.com/openfoodfacts/openfoodfacts-explorer) (Svelte) ([What can I work on ?](https://github.com/openfoodfacts/openfoodfacts-explorer/issues/2))
* [Open Food Facts Web Components](https://github.com/openfoodfacts/openfoodfacts-webcomponents) (Lit) ([What can I work on ?](https://github.com/openfoodfacts/openfoodfacts-webcomponents/issues/29))

### 🥫 Server
* [Server](https://github.com/openfoodfacts/openfoodfacts-server) (Perl, HTML, CSS, Dockerized) ([What can I work on ?](https://github.com/openfoodfacts/openfoodfacts-server/issues/5529)) ([Server-side project management](https://github.com/orgs/openfoodfacts/projects/12))
* [Content pages for the web server](https://github.com/openfoodfacts/openfoodfacts-web) (HTML/CSS) ([What can I work on ?](https://github.com/openfoodfacts/openfoodfacts-web/issues/206))

### 🔖 Folksonomy Engine - K/V system to describe products in a flexible way - Especially useful for Open Products Facts.
* [Folksonomy Engine cross platform project management](https://github.com/orgs/openfoodfacts/projects/5)
* [Folksonomy Engine](https://github.com/openfoodfacts/folksonomy_api) (Python/FastAPI) ([Server](https://github.com/openfoodfacts/folksonomy_api) ([What can I work on ?](https://github.com/openfoodfacts/folksonomy_api/issues/70))
* [Demo app](https://github.com/openfoodfacts/folksonomy_mobile_experiment) ([What can I work on ?](https://github.com/openfoodfacts/folksonomy_mobile_experiment/issues/7))
* [Frontend](https://github.com/openfoodfacts/folksonomy_frontend) ([What can I work on ?](https://github.com/openfoodfacts/folksonomy_frontend/issues/19) )

### Infrastructure
* [Infrastructure](https://github.com/openfoodfacts/openfoodfacts-infrastructure) ([What can I work on ?](https://github.com/openfoodfacts/openfoodfacts-infrastructure/issues/88)) / [Monitoring](https://github.com/openfoodfacts/openfoodfacts-monitoring) ([What can I work on ?](https://github.com/openfoodfacts/openfoodfacts-monitoring/issues/1)) ([Infrastructure & Monitoring project management](https://github.com/orgs/openfoodfacts/projects/19)

### 🔎 Search
* [Search-a-licious - New search backend using Elastic](https://github.com/openfoodfacts/search-a-licious) ([What can I work on ?](https://github.com/openfoodfacts/search-a-licious/issues/1))

### 💸 Prices
* [Open Prices backend](https://github.com/openfoodfacts/open-prices) ([What can I work on ?](https://github.com/openfoodfacts/open-prices/issues/39))
* [Open Prices frontend](https://github.com/openfoodfacts/open-prices-frontend) ([What can I work on ?](https://github.com/openfoodfacts/open-prices-frontend/issues/220))

### 🤳🥫 Mobile
* [New official app (Android/iPhone) in Flutter - Codename Smoothie](https://github.com/openfoodfacts/smooth-app) (Cross platform: Dart/Flutter) ([What can I work on ?](https://github.com/openfoodfacts/smooth-app/issues/525)) ([Mobile app project management](https://github.com/orgs/openfoodfacts/projects/7)

### 🤖 Machine learning 
* [Artificial intelligence project management - Cross repository](https://github.com/orgs/openfoodfacts/projects/16)
* [Robotoff](https://github.com/openfoodfacts/robotoff) (Python, Dockerized) ([What can I work on ?](https://github.com/openfoodfacts/robotoff/issues/374))
* [Open Food Facts AI - research](https://github.com/openfoodfacts/openfoodfacts-ai) (Python, mostly) ([What can I work on ?](https://github.com/openfoodfacts/openfoodfacts-ai/issues/76))
  
### Various tools
* [Hunger Games](https://github.com/openfoodfacts/hunger-games) (React) ([What can I work on ?](https://github.com/openfoodfacts/hunger-games/issues/37)) ([Hunger Games project management](https://github.com/orgs/openfoodfacts/projects/40))
* [Knowledge Panels for Facets](https://github.com/openfoodfacts/facets-knowledge-panels/) (Python/FastAPI) ([What can I work on ?](https://github.com/openfoodfacts/facets-knowledge-panels/issues/14))
* 🧬 [Taxonomy Editor](https://github.com/openfoodfacts/taxonomy-editor) (Python/Neo4J) ([What can I work on ?](https://github.com/openfoodfacts/taxonomy-editor/issues/30))
* [DriveOFF - Chrome and Firefox extension for e-commerce](https://github.com/openfoodfacts/DriveOFF) (Pure JS) ([What can I work on ?](https://github.com/openfoodfacts/DriveOFF/issues/4))
* [Gamification](https://github.com/openfoodfacts/openfoodfacts-events) (Python/FastAPI) ([What can I work on ?](https://github.com/openfoodfacts/openfoodfacts-events/issues/33))

### 🧽 Data quality tools
* [Power User Script - enhance OFF on Chrome and Firefox](https://github.com/openfoodfacts/power-user-script) (JS) ([What can I work on ?](https://github.com/openfoodfacts/power-user-script/issues/64))
* Nutri-Patrol ([frontend](https://github.com/openfoodfacts/nutripatrol-frontend) - [backend](https://github.com/openfoodfacts/nutripatrol)) ([What can I work on (backend) ?](https://github.com/openfoodfacts/nutripatrol/issues/24)) - ([What can I work on (frontend) ?](https://github.com/openfoodfacts/nutripatrol-frontend/issues/71))
* [Wiki page](https://wiki.openfoodfacts.org/Data_quality)

### Language SDKs
* Consolidated issue management for all SDKs: https://github.com/orgs/openfoodfacts/projects/26/views/1
* [JavaScript / NodeJS](https://github.com/openfoodfacts/openfoodfacts-nodejs) ([What can I work on ?](https://github.com/openfoodfacts/openfoodfacts-nodejs/issues/182)) - [React Native code snippet](https://github.com/openfoodfacts/openfoodfacts-react-native) ([What can I work on ?](https://github.com/openfoodfacts/openfoodfacts-react-native/issues/3)) - 🐍 [Python](https://github.com/openfoodfacts/openfoodfacts-python) ([What can I work on ?](https://github.com/openfoodfacts/openfoodfacts-python/issues/76)) - [Minimal Python Server](https://github.com/openfoodfacts/openfoodfacts-apirestpython) ([What can I work on ?](https://github.com/openfoodfacts/openfoodfacts-apirestpython/issues/76)) - ☕️ [Java](https://github.com/openfoodfacts/openfoodfacts-java) ([and its demo](https://github.com/openfoodfacts/openfoodfacts-java-demo)) ([What can I work on ?](https://github.com/openfoodfacts/openfoodfacts-java/issues/3)) ([What can I work on ?](https://github.com/openfoodfacts/openfoodfacts-java-demo/issues/2)) - [Go](https://github.com/openfoodfacts/openfoodfacts-go) ([What can I work on ?](https://github.com/openfoodfacts/openfoodfacts-go/issues/44)) - 🦀 [RUST](https://github.com/openfoodfacts/openfoodfacts-rust) ([What can I work on ?](https://github.com/openfoodfacts/openfoodfacts-rust/issues/8)) 🐘 [PHP](https://github.com/openfoodfacts/openfoodfacts-php) ([What can I work on ?](https://github.com/openfoodfacts/openfoodfacts-php/issues/39)) - [Laravel](https://github.com/openfoodfacts/openfoodfacts-laravel) ([What can I work on ?](https://github.com/openfoodfacts/openfoodfacts-laravel/issues/12)) [Dart](https://github.com/openfoodfacts/openfoodfacts-dart) ([What can I work on ?](https://github.com/openfoodfacts/openfoodfacts-dart/issues/218)) - [R](https://github.com/openfoodfacts/r-dashboard) ([What can I work on ?](https://github.com/openfoodfacts/r-dashboard/issues/2)) - 💎 [Ruby](https://github.com/openfoodfacts/openfoodfacts-ruby) ([What can I work on ?](https://github.com/openfoodfacts/openfoodfacts-ruby/issues/76)) - [Ruby (OBF)](https://github.com/openfoodfacts/openbeautyfacts-ruby) ([What can I work on ?](https://github.com/openfoodfacts/openbeautyfacts-ruby/issues/46)) - [Elixir](https://github.com/openfoodfacts/openfoodfacts-elixir)  ([What can I work on ?](https://github.com/openfoodfacts/openfoodfacts-elixir/issues/9)) - [Kotlin](https://github.com/openfoodfacts/openfoodfacts-kotlin) ([What can I work on ?](https://github.com/openfoodfacts/openfoodfacts-kotlin/issues/1)) - [Swift](https://github.com/openfoodfacts/openfoodfacts-swift) ([What can I work on ?](https://github.com/openfoodfacts/openfoodfacts-swift/issues/1))


### 📚 Documentation
* [API documentation project management](https://github.com/orgs/openfoodfacts/projects/25)

### 📢 Marketing
* 📢 Marketing coordination - [Project management](https://github.com/orgs/openfoodfacts/projects/24) - [Wiki page](https://wiki.openfoodfacts.org/Marketing)

### Old - Unsupported mobile apps (classic and experimental)
* [Classic app - Android](https://github.com/openfoodfacts/openfoodfacts-androidapp) (Kotlin) ([What can I work on ?](https://github.com/openfoodfacts/openfoodfacts-androidapp/issues/4169)) ([Classic app - Android project management](https://github.com/orgs/openfoodfacts/projects/22) - [Classic app - iPhone](https://github.com/openfoodfacts/openfoodfacts-ios) (Swift) ([What can I work on ?](https://github.com/openfoodfacts/openfoodfacts-ios/issues/912)) ([Classic app - iPhone project management](https://github.com/orgs/openfoodfacts/projects/23) - [Ubuntu Mobile](https://github.com/openfoodfacts/openfoodfacts-ubuntu) ([What can I work on ?](https://github.com/openfoodfacts/openfoodfacts-ubuntu/issues/37)) - [Vintage app - FirefoxOS](https://github.com/openfoodfacts/openfoodfacts-ffos) - [Vintage app - Cordova](https://github.com/openfoodfacts/openfoodfacts-cordova-app) - [Vintage app - Cordova - Open Beauty Facts](https://github.com/openfoodfacts/openbeautyfacts-cordova-app) - [Folksonomy mobile demo app](https://github.com/openfoodfacts/folksonomy_mobile_experiment) ([What can I work on ?](https://github.com/openfoodfacts/folksonomy_mobile_experiment/issues/7))

### Non Food products
* 🧴 Cosmetics - [Open Beauty Facts project management](https://github.com/orgs/openfoodfacts/projects/9) - [Wiki page](https://wiki.openfoodfacts.org/Open_Beauty_Facts)
* 📸 Products - [Open Products Facts project management](https://github.com/orgs/openfoodfacts/projects/43) - [Wiki page](https://wiki.openfoodfacts.org/Open_Products_Facts)
* 🐾 Pet Food - [Open Pet Food Facts project management](https://github.com/orgs/openfoodfacts/projects/36) - [Wiki page](https://wiki.openfoodfacts.org/Open_Pet_Food_Facts)

### Who do I talk to?

* Join our discussion room at <https://slack.openfoodfacts.org/> Make sure to join the appropriate channels. We will be around to help you get started 😊.

## 🙏 Contributors

This project exists thanks to all the people who contribute.


## 🙏 Backers

Thank you to all our backers! 🙏 [[Become a backer](https://opencollective.com/openfoodfacts-server#backer)]

<a href="https://opencollective.com/openfoodfacts-server#backers" target="_blank"><img src="https://opencollective.com/openfoodfacts-server/backers.svg?width=890"></a>

## 🙏 Sponsors

Support this project by becoming a sponsor. Your logo will show up here with a link to your website. [[Become a sponsor](https://opencollective.com/openfoodfacts-server#sponsor)]

<a href="https://opencollective.com/openfoodfacts-server/sponsor/0/website" target="_blank"><img src="https://opencollective.com/openfoodfacts-server/sponsor/0/avatar.svg"></a>
<a href="https://opencollective.com/openfoodfacts-server/sponsor/1/website" target="_blank"><img src="https://opencollective.com/openfoodfacts-server/sponsor/1/avatar.svg"></a>
<a href="https://opencollective.com/openfoodfacts-server/sponsor/2/website" target="_blank"><img src="https://opencollective.com/openfoodfacts-server/sponsor/2/avatar.svg"></a>
<a href="https://opencollective.com/openfoodfacts-server/sponsor/3/website" target="_blank"><img src="https://opencollective.com/openfoodfacts-server/sponsor/3/avatar.svg"></a>
<a href="https://opencollective.com/openfoodfacts-server/sponsor/4/website" target="_blank"><img src="https://opencollective.com/openfoodfacts-server/sponsor/4/avatar.svg"></a>
<a href="https://opencollective.com/openfoodfacts-server/sponsor/5/website" target="_blank"><img src="https://opencollective.com/openfoodfacts-server/sponsor/5/avatar.svg"></a>
<a href="https://opencollective.com/openfoodfacts-server/sponsor/6/website" target="_blank"><img src="https://opencollective.com/openfoodfacts-server/sponsor/6/avatar.svg"></a>
<a href="https://opencollective.com/openfoodfacts-server/sponsor/7/website" target="_blank"><img src="https://opencollective.com/openfoodfacts-server/sponsor/7/avatar.svg"></a>
<a href="https://opencollective.com/openfoodfacts-server/sponsor/8/website" target="_blank"><img src="https://opencollective.com/openfoodfacts-server/sponsor/8/avatar.svg"></a>
<a href="https://opencollective.com/openfoodfacts-server/sponsor/9/website" target="_blank"><img src="https://opencollective.com/openfoodfacts-server/sponsor/9/avatar.svg"></a>
