# Empiresmod Website

Hugo generated website. Uses the [extended version of hugo](https://github.com/gohugoio/hugo/releases) to compile SCSS.

### Install

Ensure you have hugo up and running as per their [installation guide](https://gohugo.io/getting-started/installing).

Run `hugo server -D`.

In the browser go to `http://localhost:1313`.

### Adding new posts

Follow the folder structure already in place in `content/posts/`. Add all images, videos, etc. to that folder.

When using images use the shortcode `{{< image-resize YOUR-IMAGE.EXT >}}` so that hugo can automatically resize it.

The site automatically regenerates itself using github actions when a push or merge happens on `master`.

### Autoated Tweeting

A tweet will be automatically sent out if the last git commit message to the master branch has `Tweet: ` anywhere in the message. Anything after that search expression will be posted as a tweet. Include the url if you are linking a blog post (it will be `https://www.empiresmod.com/posts/<name of folder that contains post>`)