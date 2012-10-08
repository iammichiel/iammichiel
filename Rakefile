
# Run Jekyll with options
# ~~~
desc "Cleans generated files and rebuilds site"
def jekyll(opts="", path="")
  sh "rm -rf _site"
  sh path + "jekyll --pygments " + opts
end

# Default task : start jekyll locally.
# ~~~
desc "Serve on Localhost with port 4000"
task :default do
  jekyll "--server --auto"
end


# Builds a new version of the site
# ~~~
desc "Build site using Jekyll"
task :build do
  jekyll
end

# Deploys blogs online
# ~~~
desc "Deploy blog on server."
task :deploy => ["build"] do
	sh "rsync -rv --delete _site/ njord:/var/www/iammichiel"
end