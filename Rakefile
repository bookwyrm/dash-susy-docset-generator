require 'nokogiri'

DOCSET = "Susy.docset"

namespace :docs do
  task :unpack do
    sh "git submodule update --init"
  end

  task :build_html do
    Dir.chdir('susy/docs') do
      sh "make html"
    end
  end

  task :prep_docset do
    mkdir DOCSET unless Dir.exists?(DOCSET)
    
    %w(icon.png icon@2x.png).each do |icon|
      cp icon, DOCSET
    end

    dir_contents = "#{DOCSET}/Contents"
    mkdir dir_contents unless Dir.exists?(dir_contents)
    cp "Info.plist", dir_contents

    dir_resources = "#{dir_contents}/Resources"
    mkdir dir_resources unless Dir.exists? dir_resources

    dir_documents = "#{dir_resources}/Documents"
    rm_rf dir_documents
    cp_r "susy/docs/_build/html", dir_documents
  end

  task :clean do
    rm_rf 'susy'
    rm_rf DOCSET
  end

  desc 'generate a .docset for Susy'
  task :generate => [:clean, :unpack, :build_html, :prep_docset]
end

task :default => "docs:generate"
