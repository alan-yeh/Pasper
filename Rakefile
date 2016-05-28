#############################################################################
#
# Modified version of jekyllrb Rakefile
# https://github.com/jekyll/jekyll/blob/master/Rakefile
#
#############################################################################

require 'rake'
require 'date'
require 'yaml'

CONFIG = YAML.load(File.read('_config.yml'))

DEST_DIR = CONFIG["destination"]

namespace :site do
	desc "Generate the site and push changes to remote origin"
	task :deploy do
		# Detect pull request
		if ENV['TRAVIS_PULL_REQUEST'].to_s.to_i > 0
			puts 'Pull request detected. Not proceeding with deploy.'
			exit
		end

		# Make sure destination folder exists as git repo
		sh "git clone https://$GIT_NAME:$GH_TOKEN@github.com/$GIT_NAME/$GIT_NAME.github.io.git #{DEST_DIR}"

		# Generate the site
		sh "bundle exec jekyll build"

		# Commit and push to github
		sha = `git log`.match(/[a-z0-9]{40}/)[0]
		Dir.chdir(DEST_DIR) do
			sh "git config user.name $GIT_NAME"
			sh "git config user.email $GIT_EMAIL"
			sh "git config push.default simple"
			
			sh "git add --all ."
			sh "git commit -m 'Auto update by Travis-CI @#{sha}.'"
			sh "git push"
		end
	end
end
