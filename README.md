# Steps on publishing the pages

## Build your local Jekyll site
* Navigate into the root directory of your local Jekyll site repository. The git repository is [here](https://github.com/Carlos-Liu/Github-pages.git)
* Run your Jekyll site locally
	    ```bundle exec jekyll serve```
* Preview your local Jekyll site in your web browser at the address specified in the output of above command	

* Copy all the contents from the generated folder **_site**, then paste them to the local repository (*X:\Private\GitHubBlog\standalong-pages\carlos-liu.github.io*)

* In Git Bash, run command
``` 
	git add -A
	git commit -m "comments"
	git push origin master
```
	
# Note
## Keeping your site up to date with the GitHub Pages gem
* Open Git Bash.
* Run this update command:

    * If you followed our setup recommendations and installed Bundler, run ```bundle update github-pages``` or simply ```bundle update``` and all your gems will update to the latest versions.

    * If you don't have Bundler installed, run ```gem update github-pages```

# References
> [Build your local jekyll site](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/#step-4-build-your-local-jekyll-site)

> [Online markdown editor](https://markable.in/editor/)