require 'nokogiri'

DOCSET = "Susy.docset"
DB = "#{DOCSET}/Contents/Resources/docSet.dsidx"

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

  task :index do
    def sql(*cmd)
      IO.popen("sqlite3 #{DB}", 'w+') do |sql|
        cmd.each { |l| sql << l }
      end
    end

    def index name, type, path
      if type == 'Function/mixin'
        sql("INSERT OR IGNORE INTO searchIndex(name, type, path) VALUES ('#{name}', 'Function', '#{path}');")
        sql("INSERT OR IGNORE INTO searchIndex(name, type, path) VALUES ('#{name}', 'Mixin', '#{path}');")
      else
        sql("INSERT OR IGNORE INTO searchIndex(name, type, path) VALUES ('#{name}', '#{type}', '#{path}');")
      end     
    end

    def normalize_type type
      type = 'shortcut' if type == 'shorthand'
      type.capitalize
    end

    def normalize_name name
      name.gsub(/¶$/, '')
    end

    def get_title section
      section.css('h2,h3').first
    end

    def parse_section section, file
      inner_section = section.css('.section')
      title = get_title section
      return if title.nil?

      anchor = section.css('>span:first')
      return if anchor.empty?

      if inner_section.length > 0
        type = 'Section'
      else
        descname = section.css('>.describe .descname')
        return if descname.empty?

        type = descname.text
      end

      name = title.text
      path = file + "#" + anchor.first['id']

      index normalize_name(name), normalize_type(type), path
    end

    rm DB if File.exists?(DB)
    sql "CREATE TABLE searchIndex(id INTEGER PRIMARY KEY, name TEXT, type TEXT, path TEXT);",
      "CREATE UNIQUE INDEX anchor ON searchIndex (name, type, path);"

    %w(Settings Shorthand Toolkit).each do |modname|
      file = "#{modname.downcase}.html"
      index modname, "Guide", file

      parsed = Nokogiri::HTML(File.read("#{DOCSET}/Contents/Resources/Documents/#{file}"))

      parsed.css('.section').each do |section|
        parse_section section, file
      end
    end
  end

  task :clean do
    rm_rf 'susy'
    rm_rf DOCSET
  end

  desc 'generate a .docset for Susy'
  task :generate => [:clean, :unpack, :build_html, :prep_docset]
end

task :default => "docs:generate"
