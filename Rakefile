#!/usr/bin/env ruby

require 'find'
require 'fileutils'

def generate_word
	(0...8).map { (65 + rand(26)).chr }.join
end

def create_file(path, extension)
  dir = File.dirname(path)

  unless File.directory?(dir)
    FileUtils.mkdir_p(dir)
  end

  path << ".#{extension}"
  File.open(path, 'w') do |page|
    page << "word = " + generate_word
  end
end


desc "dircommit file"
task :dircommit do
	commitdir = "dircommit"
	FileUtils.cd(commitdir) do |path|
		dir = File.join(File.dirname(__FILE__), path, "*")
		subdir = File.join(File.dirname(__FILE__), path)
		Dir[dir].each { |x|
			if FileTest.directory?(x)
				cmdir = x.sub(File.join(File.dirname(__FILE__), commitdir, "/"), "")
				# puts x
				puts "\n add directory #{cmdir}"
				system "git add -A #{cmdir}/"
				message = "Site updated #{cmdir} directory at #{Time.now.utc}"
				puts "\n## Committing: #{message}"
				system "git commit -m \"#{message}\""
				puts "\n## Pushing generated #{cmdir} website"
				Bundler.with_clean_env { system "git push origin master" }
				puts "\n## Github Pages deploy complete"
				# FileUtils.cd(x) do |dir|  # chdir
				# end
			end
		}
		puts "\n add directory #{commitdir}"
		system "git add -A ."
		message = "Site updated #{commitdir} directory at #{Time.now.utc}"
		puts "\n## Committing: #{message}"
		system "git commit -m \"#{message}\""
		puts "\n## Pushing generated #{commitdir} website"
		Bundler.with_clean_env { system "git push origin master" }
		puts "\n## Github Pages deploy complete"
	end
end


desc "generate file"
task :generate do
	dircommitglob = Dir.glob(File.join(File.dirname(__FILE__), "dircommit", "*"))
	FileUtils.rm_rf(dircommitglob)
	FileUtils.cd("dircommit/") do
		10.times do
			p1 = generate_word
			p2 = generate_word
			create_file(File.join(p1, p2, generate_word), 'txt')
			2.times do
				create_file(File.join(p1, generate_word), 'txt')
			end
		end
		2.times do
			create_file(File.join(generate_word), 'txt')
		end

	end
end


desc "Task description"
task :default do
	
end