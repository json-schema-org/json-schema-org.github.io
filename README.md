---
📌 **Deprecation Notice** 📌

This repository is now deprecated. To contribute to JSON Schema's website please use the new repository ➡️ [https://github.com/json-schema-org/website](https://github.com/json-schema-org/website).

---

# JSON Schema Website

[![Contributor Covenant](https://img.shields.io/badge/Contributor%20Covenant-2.1-4baaaa.svg)](https://github.com/json-schema-org/.github/blob/main/CODE_OF_CONDUCT.md)
[![Project Status: Moved/Deprecated to https://github.com/json-schema-org/website – The project has been moved to a new location, and the version at that location should be considered authoritative.](https://www.repostatus.org/badges/latest/moved.svg)](https://www.repostatus.org/#moved) to [https://github.com/json-schema-org/website](https://github.com/json-schema-org/websitem)
[![Financial Contributors on Open Collective](https://opencollective.com/json-schema/all/badge.svg?label=financial+contributors)](https://opencollective.com/json-schema) 

This is the repository for the [JSON Schema website](https://json-schema.org).

For issues, discussion, and changes to the JSON Schema specification, please use the [json-schema-spec](https://github.com/json-schema-org/json-schema-spec) repository.

## Status
For the current status of issues and pull requests, please see the following badges

[![Issues](https://img.shields.io/github/issues-raw/json-schema-org/json-schema-org.github.io.svg)](https://github.com/json-schema-org/json-schema-org.github.io/issues)

[![Available](https://img.shields.io/github/issues/json-schema-org/json-schema-org.github.io/Status:%20Available.svg?color=brightgreen)](https://github.com/json-schema-org/json-schema-org.github.io/issues?q=is%3Aopen+is%3Aissue+label%3A%22Status%3A+Available%22) [![In Progress](https://img.shields.io/github/issues/json-schema-org/json-schema-org.github.io/Status:%20In%20Progress.svg)](https://github.com/json-schema-org/json-schema-org.github.io/labels/Status:%20In%20Progress) [![Review Needed](https://img.shields.io/github/issues/json-schema-org/json-schema-org.github.io/Status:%20Review%20Needed.svg)](https://github.com/json-schema-org/json-schema-org.github.io/labels/Status%3A%20Review%20Needed)

[![Critical](https://img.shields.io/github/issues/json-schema-org/json-schema-org.github.io/Priority:%20Critical.svg?color=critical
)](https://github.com/json-schema-org/json-schema-org.github.io/labels/Priority%3A%20Critical) [![High](https://img.shields.io/github/issues/json-schema-org/json-schema-org.github.io/Priority:%20High.svg?color=important)](https://github.com/json-schema-org/json-schema-org.github.io/labels/Priority%3A%20High) [![Medium](https://img.shields.io/github/issues/json-schema-org/json-schema-org.github.io/Priority:%20Medium.svg)](https://github.com/json-schema-org/json-schema-org.github.io/labels/Priority%3A%20Medium) [![Low](https://img.shields.io/github/issues/json-schema-org/json-schema-org.github.io/Priority:%20Low.svg)](https://github.com/json-schema-org/json-schema-org.github.io/labels/Priority%3A%20Low)

Labels are assigned based on [Sensible Github Labels](https://github.com/Relequestual/sensible-github-labels).

## Compile and run locally

This site runs via github pages, with automatically building PR previews via netlify.

This project uses git submodules, so you will need to run the following commands
to fully clone the repo.

```bash
git submodule init
git submodule update
```

You can run the site locally using `docker-compose up` and browse to
http://localhost:4000

## License

The source material in this repository is licensed under the AFL or BSD license.
