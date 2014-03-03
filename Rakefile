
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
task :default => ["less"] do
  jekyll "serve --watch"
end

# Compile les fichiers less
# ~~~
desc "Compile the less files"
task :less do 
	sh "lessc less/style.less > assets/stylesheets/style.css"
end

# Builds a new version of the site
# ~~~
desc "Build site using Jekyll"
task :build => ["less"] do
  jekyll "build"
end

# Deploys blogs online
# ~~~
desc "Deploy blog on server."
task :deploy => ["build"] do
	sh "rsync -rv --delete _site/ pink:/var/www/iammichiel"
end
