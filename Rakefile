
# Run Jekyll with options
# ~~~
desc "Cleans generated files and rebuilds site"
def jekyll(opts="", path="")
  sh "rm -rf _site"
  sh path + "jekyll " + opts
end

# Default task : start jekyll locally.
# ~~~
desc "Serve on Localhost with port 4000"
task :default do
  jekyll "serve --watch"
end


# Builds a new version of the site
# ~~~
desc "Build site using Jekyll"
task :build do
  jekyll "build"
end

# Deploys blogs online
# ~~~
desc "Deploy blog on server."
task :deploy => ["build"] do
	sh "rsync -rv --delete _site/ anivia:/var/www/iammichiel"
end