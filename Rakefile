require 'nokogiri'

namespace :docs do
  task :unpack do
    sh "git submodule update --init"
  end

  task :build_html do
    Dir.chdir('susy/docs') do
      sh "make html"
    end
  end

  task :clean do
    rm_rf 'susy'
  end

  desc 'generate a .docset for Susy'
  task :generate => [:unpack, :build_html]
end

task :default => "docs:generate"
