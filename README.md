# typisttech/image-optimize-command

[![Latest Stable Version](https://poser.pugx.org/typisttech/image-optimize-command/v/stable)](https://packagist.org/packages/typisttech/image-optimize-command)
[![Total Downloads](https://poser.pugx.org/typisttech/image-optimize-command/downloads)](https://packagist.org/packages/typisttech/image-optimize-command)
[![Build Status](https://travis-ci.org/TypistTech/image-optimize-command.svg?branch=master)](https://travis-ci.org/TypistTech/image-optimize-command)
[![PHP Versions Tested](http://php-eye.com/badge/typisttech/image-optimize-command/tested.svg)](https://travis-ci.org/TypistTech/image-optimize-command)
[![StyleCI](https://styleci.io/repos/119003751/shield?branch=master)](https://styleci.io/repos/119003751)
[![Dependency Status](https://gemnasium.com/badges/github.com/TypistTech/image-optimize-command.svg)](https://gemnasium.com/github.com/TypistTech/image-optimize-command)
[![License](https://poser.pugx.org/typisttech/image-optimize-command/license)](https://packagist.org/packages/typisttech/image-optimize-command)
[![Donate via PayPal](https://img.shields.io/badge/Donate-PayPal-blue.svg)](https://typist.tech/donate/image-optimize-command/)
[![Hire Typist Tech](https://img.shields.io/badge/Hire-Typist%20Tech-ff69b4.svg)](https://typist.tech/contact/)

Easily optimize images using WP CLI.

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->


- [Using](#using)
- [Installing](#installing)
  - [Optimization tools](#optimization-tools)
- [FAQs](#faqs)
  - [What kind of optimization it does?](#what-kind-of-optimization-it-does)
  - [Why the optimize command stopped for no reason?](#why-the-optimize-command-stopped-for-no-reason)
  - [Does running `wp image-optimize run` multiple times trigger multiple optimization for the same attachments?](#does-running-wp-image-optimize-run-multiple-times-trigger-multiple-optimization-for-the-same-attachments)
  - [Why my GIFs stopped animating?](#why-my-gifs-stopped-animating)
  - [Is it for everyone?](#is-it-for-everyone)
- [Support](#support)
  - [Why don't you hire me?](#why-dont-you-hire-me)
  - [Want to help in other way? Want to be a sponsor?](#want-to-help-in-other-way-want-to-be-a-sponsor)
- [Credits](#credits)
- [Contributing](#contributing)
  - [Reporting a bug](#reporting-a-bug)
  - [Creating a pull request](#creating-a-pull-request)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

WP CLI wrapper for [spatie/image-optimizer](https://github.com/spatie/image-optimizer). Optimizing PNGs, JPGs, SVGs and GIFs by running them through a chain of various image [optimization tools](#Optimization-tools).

## Using

```bash
# Optimize 10 attachments
$ wp image-optimize run --limit=10

# Optimize after thumbnail regeneration
$ wp media regenerate --yes
$ wp image-optimize reset --yes
$ wp image-optimize run --limit=9999999
```

## Installing

Installing this package requires WP-CLI v1.4.1 or greater. Update to the latest stable release with `wp cli update`.

Once you've done so, you can install this package with:

    wp package install https://github.com/TypistTech/image-optimize-command.git

### Optimization tools

Under the hood, `image-optimize-command` invokes [spatie/image-optimizer](https://github.com/spatie/image-optimizer) which requires these binaries installed:

- [JpegOptim](http://freecode.com/projects/jpegoptim)
- [Optipng](http://optipng.sourceforge.net/)
- [Pngquant 2](https://pngquant.org/)
- [SVGO](https://github.com/svg/svgo)
- [Gifsicle](http://www.lcdf.org/gifsicle/)

Check spatie/image-optimizer's readme for [install instructions](https://github.com/spatie/image-optimizer#optimization-tools).

Note that WordPress doesn't support SVG out of the box. You can omit [SVGO](https://github.com/svg/svgo).

## FAQs

### What kind of optimization it does?

Mostly applying compression, removing metadata and reducing the number of colors to PNGs, JPGs, SVGs and GIFs. The package is smart enough pick the right tool for the right image.

Check Freek Van der Herten's [article](https://murze.be/easily-optimize-images-using-php-and-some-binaries) explaining `spatie/image-optimizer`'s [*sane default configuration*](https://github.com/spatie/image-optimizer/blob/124da0d/src/OptimizerChainFactory.php)

### Why the optimize command stopped for no reason?

Expected result:
```bash
$ wp image-optimize run --limit=10
Success: 10 unoptimized attachment(s) found. Starting...
...omitted...
Success 10 attachment(s) optimized

$ wp image-optimize run --limit=10
No unoptimized attachment found. Abort!
```

If it stopped halfway, most likely you deleted the images from disk but not updated WordPress' database. Simplest solution is to regenerate thumbnails then optimize again:
```bash
$ wp media regenerate --yes
$ wp image-optimize reset --yes
$ wp image-optimize run --limit=9999999
```

### Does running `wp image-optimize run` multiple times trigger multiple optimization for the same attachments?

No, by default, optimized flags (meta fields) are given to attachments after optimization (`$ wp image-optimize run --limit=999`). This is to prevent re-optimizing an already optimized attachment. If you changed the image files (e.g.: resize / regenerate thumbnail), you must first reset their meta flags.

### Why my GIFs stopped animating?

> When you upload an image using the media uploader, WordPress automatically creates several copies of that image in different sizes...When creating new image sizes for animated GIFs, **WordPress ends up saving only the first frame of the GIF**...
> --- [wpbeginner](http://www.wpbeginner.com/wp-tutorials/how-to-add-animated-gifs-in-wordpress/)

Luckily for you, Lasse M. Tvedt showed how to disable stop WordPress from resizing GIFs on [StackExchange](https://wordpress.stackexchange.com/a/229724).

### Is it for everyone?

No, it comes at a cost. Optimization is CPU intensive. Expect CPU usage rockets up to 100% during optimization. Schedule it to run at late night in small batches.

## Support

Love `image-optimize-command`? Help me maintain it, a [donation here](https://typist.tech/donation/) can help with it.

### Why don't you hire me?

Ready to take freelance WordPress jobs. Contact me via the contact form [here](https://typist.tech/contact/) or, via email [info@typist.tech](mailto:info@typist.tech)

### Want to help in other way? Want to be a sponsor?

Contact: [Tang Rufus](mailto:tangrufus@gmail.com)

## Credits

[`image-optimize-command`](https://github.com/TypistTech/image-optimize-command) is a [Typist Tech](https://typist.tech) project and maintained by [Tang Rufus](https://twitter.com/TangRufus), freelance developer for [hire](https://www.typist.tech/contact/).

Full list of contributors can be found [here](https://github.com/TypistTech/image-optimize-command/graphs/contributors).

Special thanks to [Freek Van der Herten](https://github.com/freekmurze/) whose [spatie/image-optimizer](https://github.com/spatie/image-optimizer) package makes this project possible.

## Contributing

We appreciate you taking the initiative to contribute to this project.

Contributing isn’t limited to just code. We encourage you to contribute in the way that best fits your abilities, by writing tutorials, giving a demo at your local meetup, helping other users with their support questions, or revising our documentation.

For a more thorough introduction, [check out WP-CLI's guide to contributing](https://make.wordpress.org/cli/handbook/contributing/). This package follows those policy and guidelines.

### Reporting a bug

Think you’ve found a bug? We’d love for you to help us get it fixed.

Before you create a new issue, you should [search existing issues](https://github.com/typisttech/image-optimize-command/issues?q=label%3Abug%20) to see if there’s an existing resolution to it, or if it’s already been fixed in a newer version.

Once you’ve done a bit of searching and discovered there isn’t an open or fixed issue for your bug, please [create a new issue](https://github.com/typisttech/image-optimize-command/issues/new). Include as much detail as you can, and clear steps to reproduce if possible. For more guidance, [review our bug report documentation](https://make.wordpress.org/cli/handbook/bug-reports/).

### Creating a pull request

Want to contribute a new feature? Please first [open a new issue](https://github.com/typisttech/image-optimize-command/issues/new) to discuss whether the feature is a good fit for the project.

Once you've decided to commit the time to seeing your pull request through, [please follow our guidelines for creating a pull request](https://make.wordpress.org/cli/handbook/pull-requests/) to make sure it's a pleasant experience. See "[Setting up](https://make.wordpress.org/cli/handbook/pull-requests/#setting-up)" for details specific to working on this package locally.