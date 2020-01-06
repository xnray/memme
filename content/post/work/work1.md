
+++
showonlyimage = false
draft = false
#image = "img/portfolio/a4-paper.jpg"
date = "2020-01-6T18:25:22+05:30"
title = "将hugo生成的页面托管到github pages"
writer = "练法术的兔子"
categories = [ "web"]
weight = 1
+++

## GitHub User or Organization Pages

1.   Create a `<YOUR-PROJECT>` (e.g. `blog`) repository on GitHub. This repository will contain Hugo's content and other source files.
2.   Create a `<USERNAME>.github.io` GitHub repository. This is the repository that will contain the fully rendered version of your Hugo website.
3.   `git clone <YOUR-PROJECT-URL> && cd <YOUR-PROJECT>`
4.   Paste your existing Hugo project into a new local `<YOUR-PROJECT>` repository. Make sure your website works locally (`hugo server` or `hugo server -t <YOURTHEME>`) and open your browser to [http://localhost:1313](http://localhost:1313).
5.   Once you are happy with the results:
    *   Press Ctrl+C to kill the server
    *   Before proceeding run `rm -rf public` to completely remove the `public` directory
6.   `git submodule add -b master git@github.com:<USERNAME>/<USERNAME>.github.io.git public`. This creates a git [submodule](https://github.com/blog/2104-working-with-submodules). Now when you run the `hugo` command to build your site to `public`, the created `public` directory will have a different remote origin (i.e. hosted GitHub repository).

## Put it Into a Script

You're almost done. In order to automate next steps create a `deploy.sh`script. You can also make it executable with `chmod +x deploy.sh`.

The following are the contents of the `deploy.sh` script:
```
#!/bin/sh

# If a command fails then the deploy stops
set -e

printf "\033[0;32mDeploying updates to GitHub...\033[0m\n"

# Build the project.
hugo # if using a theme, replace with `hugo -t <YOURTHEME>`

# Go To Public folder
cd public

# Add changes to git.
git add .

# Commit changes.
msg="rebuilding site $(date)"
if [ -n "$*" ]; then
	msg="$*"
fi
git commit -m "$msg"

# Push source and build repos.
git push origin master
```
You can then run ./deploy.sh "Your optional commit message" to send changes to <USERNAME>.github.io. Note that you likely will want to commit changes to your <YOUR-PROJECT> repository as well.

That's it! Your personal page should be up and running at https://<USERNAME>.github.io within a couple minutes.

## Add .nojekyll to public dirctory
- GitHub Pages will use Jekyll to build your site by default. If you want to use a static site generator other than Jekyll, disable the Jekyll build process by creating an empty file called .nojekyll in the root of your publishing source, then follow your static site generator's instructions to build your site locally.


   

   

   

