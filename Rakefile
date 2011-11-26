require 'rubygems'
require 'active_support/inflector'
require 'iconv'

include ActiveSupport::Inflector

task :default => :import

desc "Import  Books"
task :import do

	source_directory = "./_books"
	target_directory = "books"
	include_directory = "./_includes"

	ic = Iconv.new('UTF-8//IGNORE', 'UTF-8')

	# Get the list of books
	book_files = Dir.glob("#{source_directory}/*")

	book_list = ""

	# For each book
	book_files.each do |path| 

		filename = path.split("/")[2]
		
		# Get the title
		title = filename.split(".txt")[0]
		new_filename = "#{title.parameterize}.md"
		new_path = "#{target_directory}/#{new_filename}"

		puts new_path

		# Get the contents
		text_file = File.open(path, "rb")
		contents = text_file.read
		text_file.close

		new_contents = "---
layout: book
title: #{title}
---
#{contents}
"

		# Jekyll doesn't like non-UTF-8 characters
		new_contents = ic.iconv(new_contents)

		new_contents.gsub!(/\[pagebreak\]/, "")
	
		new_contents.gsub!(/<p align="left">/, "\n\n")

		new_contents.gsub!(/<img src='img:\/\/Textures\/Interface\/Books\/Illuminated_Letters\//, "")
		new_contents.gsub!(/_letter.png'>/, "")
	
		# Write out a file
		markdown_file = File.open(new_path, "w")
		markdown_file.write(new_contents)
		markdown_file.close

		if !title.start_with?("123")
			book_list += "* [#{title}](books/#{title.parameterize})\n"
		end	

	end

	include_file = File.open("#{include_directory}/books.md", "w")
	include_file.write(book_list)
	include_file.close
	

end


