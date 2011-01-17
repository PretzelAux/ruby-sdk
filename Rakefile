require 'rake/testtask'

RUBIES = %w{ 1.9.2 1.8.7 1.8.6 rbx-1.2.0 }

Rake::TestTask.new do |test|
  test.libs   << 'test'
  test.pattern = 'test/**/test_*.rb'
  test.verbose = true
end

namespace :test do
  begin
    `rvm -v`
    
    desc 'Run tests against all supported Rubies'
    task :multiruby do
      system "rvm #{RUBIES.join(',')} ruby bundle exec rake test"
      
      require 'pathname'
      Pathname.glob('**/*.rbc').each {|path| path.unlink }
    end
    
    namespace :multiruby do
      desc 'Prepare supported rubiesfor testing'
      task :setup do
        warn 'Preparing multiruby. This may take awhile...'
        
        # create gemsets, install bundler, bundle
        RUBIES.each {|ruby| system "rvm #{ruby} gemset create transloadit" }
        system "rvm #{RUBIES.join(',')} gem install bundler --no-ri --no-rdoc"
        system "rvm #{RUBIES.join(',')} ruby bundle install"
      end
    end
  rescue Errno::ENOENT
    desc 'You need `rvm` installed to test against multiple Rubies'
    task :multiruby
  end
end

begin
  require 'yard'
  require 'yard/rake/yardoc_task'
  
  YARD::Rake::YardocTask.new :doc do |yard|
    yard.options = %w{
      --title  Transloadit
      --readme README.md
    }
  end
rescue
  desc 'You need the `yard` gem to generate documentation'
  task :doc
end
