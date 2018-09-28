require 'ruby-progressbar'

desc 'Check style in your JavaScript files with ESLint'
task :eslint do
  system 'node_modules/.bin/eslint source/javascripts/*.js'
end

desc 'Check style in your Ruby files with RuboCop'
task :rubocop do
  system 'rubocop'
end

namespace :middleman do
  desc 'Run `middleman build` with bundler'
  task :build do
    building_bar = ProgressBar.create(title: '🏗  Building project', progress_mark: '.', format: '%t%B')
    10.times { building_bar.increment; sleep 0.25 }
    puts '🏗  Building project..........'
    system 'bundle exec middleman build'
  end

  desc 'Deploy Middleman application on GitHub Pages'
  task :deploy do
    remotes_bar = ProgressBar.create(title: '〰️ Looking for GitHub remotes', progress_mark: '.', format: '%t%B')
    10.times { remotes_bar.increment; sleep 0.25 }
    puts '〰️ Looking for a GitHub origin..........'
    remote = `git config --get remote.origin.url`
    if remote.include?('git@github.com')
      puts "〰️ Using #{remote}"
      gh_pages_url = "https://#{remote.sub('git@github.com:', '').sub('.git', '').sub('/', '.github.io/')}"
      `git branch -f gh-pages`
      unless ARGV[1] == 'no-build'
        system 'rake middleman:build'
        `mv build/views/* build && rm -rf build/views`
        `git add build`
        `git commit -m 'Automated Middleman deploy commit #{Time.now.strftime 'on %-d %b %Y at %H:%M:%S'}'`
        `git push origin master`
      end
      system 'git subtree push --prefix build origin gh-pages'
      puts "🚀 Website successfully published at #{gh_pages_url}"
    else
      puts '⚠️  ERROR: You must set a GitHub origin before deploying'
    end
  end
end

task default: %i[eslint rubocop]
