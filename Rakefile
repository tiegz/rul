require 'rubygems'
require 'rake'
require 'activesupport'
require 'erb'

RUL_ROOT      = File.dirname(__FILE__) + "/app"
XUL_PATH      = "/Library/Frameworks/XUL.framework"
BIN_PATH      = XUL_PATH + "/xulrunner-bin" # TODO x-platformize

# TODO ARGV these suckas
APP_NAME      = `pwd`.split('/').last.underscore.strip
APP_VERSION   = 1.0
APP_ID        = 'my@email.address'
APP_AUTHOR    = 'MyName'

TEMPLATE_ROOT = File.dirname(__FILE__), "lib", "templates"

namespace :app do
  namespace :install do
    desc "install this xulrunner application for Mac OS X"
    task :mac do
      # system %(#{BIN_PATH} --install-app #{RUL_ROOT}/app.zip) 
      
      FileUtils.mkdir_p "#{APP_NAME}.app"
      FileUtils.mkdir_p "#{APP_NAME}.app/Contents/Frameworks"
      FileUtils.mkdir_p "#{APP_NAME}.app/Contents/MacOS"
      puts " -> generated #{APP_NAME}.app/ bundle" 
      FileUtils.ln_s XUL_PATH, "#{APP_NAME}.app/Contents/Frameworks/XUL.framework"
      FileUtils.ln_s RUL_ROOT, "#{APP_NAME}.app/Contents/Resources"
      FileUtils.ln_s "#{XUL_PATH}/Versions/Current/xulrunner", "#{APP_NAME}.app/Contents/MacOS/xulrunner"
      puts " -> created symbolic links to framework, app, and ? in bundle"
      File.open "#{APP_NAME}.app/Contents/Info.plist", 'w' do |f|
        template = File.join TEMPLATE_ROOT, 'info.plist'
        f.write ERB.new(File.open(template).read, nil, '-').result(binding)
        puts " -> generated app/application.ini"
      end
      # system %(#{BIN_PATH} "#{APP_NAME}.app/Contents/Resources/application.ini")
      # puts " -> running .app"
    end
  end

  desc "removes install files and app"
  task :reset do
    FileUtils.rm_rf "app"
    FileUtils.rm_rf "#{APP_NAME}.app"
  end

  desc "run this xulrunner application"
  task :run do
    cmd = %(#{BIN_PATH} #{RUL_ROOT}) 
    puts cmd
    system cmd
  end
  
  desc "build the skeleton for this app" # TODO replace with a bin "rul" script that generates the skeleton
  task :new do
    FileUtils.mkdir_p 'app'
    puts " -> generated app/"
  
    File.open 'app/application.ini', 'w' do |f|
      template = File.join TEMPLATE_ROOT, 'application.ini'
      f.write ERB.new(File.open(template).read, nil, '-').result(binding)
      puts " -> generated app/application.ini"
    end

    FileUtils.mkdir_p 'app/chrome/content'
    puts " -> generated app/chrome/"
    puts " -> generated app/chrome/content"
    FileUtils.mkdir_p 'app/chrome/images'
    puts " -> generated app/chrome/images"
    FileUtils.mkdir_p 'app/chrome/icons/default'
    puts " -> generated app/chrome/icons"
    puts " -> generated app/chrome/icons/default"

    File.open "app/chrome/content/#{APP_NAME}.xul", 'w' do |f|
      template = File.join TEMPLATE_ROOT, 'main.xul'
      f.write ERB.new(File.open(template).read, nil, '-').result(binding)
      puts " -> generated app/chrome/content/#{APP_NAME}.xul"
    end
    File.open 'app/chrome/chrome.manifest', 'w' do |f|
      template = File.join TEMPLATE_ROOT, 'chrome.manifest'
      f.write ERB.new(File.open(template).read, nil, '-').result(binding)
      puts " -> generated app/chrome/chrome.manifest"
    end
    FileUtils.mkdir_p 'app/defaults/preferences'
    File.open "app/defaults/preferences/#{APP_NAME}-prefs.js", 'w' do |f|
      template = File.join TEMPLATE_ROOT, 'prefs.js'
      f.write ERB.new(File.open(template).read, nil, '-').result(binding)
      puts " -> generated app/defaults/preferences/#{APP_NAME}-prefs.js"
    end
  end
end

