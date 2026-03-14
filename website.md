# Contributing to the OpenRTX website

This page discusses the basic setup for contributing to the OpenRTX website.

1. [Overview](#overview)
2. [Installing dependencies](#installing-dependencies)
3. [Obtaining the source code](#obtaining-the-source-code)
4. [Building and running the website](#building-and-running-the-website)
5. [Checking your changes for typos](#checking-your-changes-for-typos)

## Overview

The OpenRTX website is a _static site_. This means that content is pre-built and packaged for viewing in a browser without the need for a complex webserver or database configuration. This makes contributing to the website relatively simple, and only requires installing a few packages, many of which you may already have if you have done any kind of development before.

OpenRTX uses the `docsify` static site generator, which converts convert Markdown files into HTML that can be rendered in a browser. This generated output is hosted on GitHub Pages.

---

## Installing dependencies

In order to build and run the website locally, you must have the following packages installed (this process may be specific to your Linux distribution or operating system):

- `git`
- `nodejs`
- `npm`

Once you have these installed, you will need to use `npm` to install the `docsify` npm package:

```bash
npm i docsify -g
```

You can also install the `cspell` package to check your changes for any spelling errors:

```bash
npm i cspell -g
```
---

## Obtaining the source code

To obtain the source code for the OpenRTX website, clone the GitHub repository using the following command:

```bash
git clone https://github.com/OpenRTX/openrtx.github.io
```
---

## Building and running the website

Once you have cloned the repository, run the following command in the parent directory of the `openrtx.github.io/` repository:

```bash
docsify serve openrtx.github.io
```
In your terminal output you will see the following line:

```bash
Listening at http://localhost:3000
```

Open the URL displayed in the terminal output (typically `http://localhost:3000` by default) to view your locally built version of the OpenRTX website.

You can now make changes to the website by editing any of the Markdown (`.md`) files in the `openrtx.github.io/` directory. When a file is modified and saved, your local version of the website should update automatically and show your changes live without needing to refresh your web browser.

---

## Checking your changes for typos
To check for spelling errors, you can run use the `cspell` command installed earlier. For example, to check for spelling errors in the file you are reading right now, you can run the following command inside the `openrtx.github.io/` directory:

```bash
cspell website.md
```
