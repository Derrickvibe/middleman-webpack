desc 'Check style in your JavaScript files with ESLint'
task :eslint do
  system 'node_modules/.bin/eslint source/assets/javascripts/*.js'
end

desc 'Check style in your Ruby files with RuboCop'
task :rubocop do
  system 'rubocop'
end

namespace :middleman do
  desc 'Run `middleman build` with bundler'
  task :build do
    system 'bundle exec middleman build'
  end

  desc 'Deploy Middleman application on GitHub Pages'
  task :deploy do
    ARGV.each { |a| task a.to_sym do ; end }
    if system 'git remote -v &>/dev/null'
      remote = `git config --get remote.origin.url`
      gh_pages_url = "https://#{remote.gsub('git@github.com:', '').gsub('/', '.github.io/')}"
      `git branch -f gh-pages`
      unless ARGV[1] == 'no-build'
        ProgressBar.create(title: '🏗  Building project', starting_at: 0, total: 10, progress_mark: '.')
        system 'rake middleman:build'
        `git add build`
        `git commit -m 'Automated Middleman deploy commit #{Time.now.strftime 'on %-d %b %Y at %H:%M:%S'}'`
        `git push origin master`
      end
      system 'git subtree push --prefix build origin gh-pages'
      puts "🚀 Website successfully published at #{gh_pages_url}"
    else
      puts '⚠️ ERROR: You must set a remote before deploying'
    end
  end
end

task default: %i[eslint rubocop]
