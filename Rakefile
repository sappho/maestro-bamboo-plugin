require 'rake/clean'
require 'rspec/core/rake_task'
require 'zippy'

$:.push File.expand_path("../src", __FILE__)

CLEAN.include("maestro-bamboo-plugin-1.0-SNAPSHOT.zip","vendor","package")

task :default => [:bundle, :spec, :package]

desc "Run specs"
RSpec::Core::RakeTask.new do |t|
  t.pattern = "./spec/**/*_spec.rb" # don't need this, it's default.
  t.rspec_opts = "--fail-fast --format p --color"
  # Put spec opts in a file named .rspec in root
end

desc "Get dependencies with Bundler"
task :bundle do
  system "bundle package"
end

def add_file( zippyfile, dst_dir, f )
  puts "Writing #{f} at #{dst_dir}"
  zippyfile["#{dst_dir}/#{f}"] = File.open(f)
end

def add_dir( zippyfile, dst_dir, d )
  glob = "#{d}/**/*"
  FileList.new( glob ).each { |f|
    if (File.file?(f))
      add_file zippyfile, dst_dir, f
    end
  }
end

desc "Package plugin zip"
task :package do
  Zippy.create 'maestro-bamboo-plugin-1.0-SNAPSHOT.zip' do |z|
    add_dir z, '.', 'vendor'
    add_file z, '.', 'manifest.json'
    add_file z, '.', 'README.md'
    add_file z, '.', 'src/bamboo_worker.rb'
  end
end