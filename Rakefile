require 'bundler'
require 'bundler/gem_tasks'
require 'bundler/setup'
require 'rubocop/rake_task'
require 'opal'

# Add our opal/ directory to the load path
Opal.append_path(File.expand_path('../lib', __FILE__))

require 'opal/rspec/rake_task'

task :docs do
  `bundle exec yardoc -r Readme.md --markup-provider=redcarpet --markup=markdown 'lib/**/*.rb' - Readme.md docs/*.md`
end

# Setup the opal:rspec task
Opal::RSpec::RakeTask.new('opal:rspec') do |s|
  # Add the app folder to the opal load path.
  s.append_path('app')
end

task default: [:test]

require 'rspec/core/rake_task'
RSpec::Core::RakeTask.new('ruby:rspec')

task :test do
  puts "--------------------------\nRun specs in Opal\n--------------------------"
  Rake::Task['opal:rspec'].invoke
  puts "--------------------------\nRun specs in normal ruby\n--------------------------"
  Rake::Task['ruby:rspec'].invoke
end

task :opal_specs_in_browser do
  require 'volt/server/websocket/rack_server_adaptor'
  require 'rack/cascade'

  server = Rack::Handler.get(RUNNING_SERVER)

  Opal::Processor.source_map_enabled = false
  app = Opal::Server.new { |s|
    s.main = 'opal/rspec/sprockets_runner'
    s.append_path 'spec'
    s.append_path 'app'
    s.debug = false
  }

  server.run(app, {})
end

# Rubocop task
RuboCop::RakeTask.new(:rubocop) do |task|
  task.options = ['--display-cop-names']
end
