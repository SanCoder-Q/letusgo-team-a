require 'active_record'
require 'logger'
require 'yaml'

desc "Migrate the database through scripts in db/."
task :migrate => :environment do
    ActiveRecord::Migrator.migrate('db/', ENV["VERSION"] ? ENV["VERSION"].to_i : nil )
end

task :environment do
    dbconfig = YAML.load(File.open("config/database.yml").read)
    env = ENV["RACK_ENV"] || "test"
    ActiveRecord::Base.establish_connection(dbconfig[env])
    ActiveRecord::Base.logger = Logger.new(File.open('database.log', 'a'))
end

if ENV["RACK_ENV"] == 'test'
	require 'rspec/core/rake_task'

	RSpec::Core::RakeTask.new :specs do |task|
		task.pattern = Dir['spec/**/*_spec.rb']
	end

	task :default => ['specs']
end

task :default => ['migrate']
