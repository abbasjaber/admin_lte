require 'pry'
require 'fileutils'
require 'bundler/gem_tasks'
require 'rspec/core/rake_task'

RSpec::Core::RakeTask.new(:spec)

task :default => :spec


desc 'Convert AdminLTE files'
task :convert do
  ENV['EXECJS_RUNTIME'] = 'Node'

  spec = Gem::Specification.find_by_name('rails-assets-admin-lte')
  dir  = "#{spec.gem_dir}/app/assets"
  puts dir

  app_path  = File.expand_path('../app', __FILE__)
  lib_path  = File.expand_path('../lib', __FILE__)
  cssjanus  = File.expand_path('../lib/to_rtl.js', __FILE__)
  style_dir = "#{app_path}/assets/stylesheets/admin_lte"
  js_dir = "#{app_path}/assets/javscripts/admin-lte"

  orig_style = "#{dir}/stylesheets/admin-lte"
  orig_js = "#{dir}/javascripts/admin-lte"

  FileUtils.rmdir "#{style_dir}/ltr"
  FileUtils.rmdir "#{style_dir}/rtl"

  Dir.glob("#{dir}/stylesheets/**/*") do |file|

    new_path = file.gsub(orig_style, "#{style_dir}/ltr/")
    rtl_path = new_path.gsub('ltr/', 'rtl/')

    FileUtils.mkdir_p File.dirname(new_path)
    FileUtils.mkdir_p File.dirname(rtl_path)

    unless File.directory? file
      puts "Source: #{file}"
      puts "LTR: #{new_path}"
      puts "RTL: #{rtl_path}"
      puts "================="

      FileUtils.cp(file, new_path)
      `nodejs #{cssjanus} #{new_path} > #{rtl_path}`
    end
  end

  FileUtils.cp("#{lib_path}/ltr/admin_lte.css",
               "#{style_dir}/ltr/admin_lte.css")
  FileUtils.cp("#{lib_path}/rtl/admin_lte.css",
               "#{style_dir}/rtl/admin_lte.css")

  # Dir.glob("#{dir}/javascripts/**/*") do |file|

  #   new_path = file.gsub(orig_js, "#{js_dir}/")

  #   FileUtils.mkdir_p File.dirname(new_path)


  #   unless File.directory? file
  #     puts "Source: #{file}"
  #     puts "DST: #{new_path}"
  #     puts "================="

  #     FileUtils.cp(file, new_path)
  #   end
  # end




end
