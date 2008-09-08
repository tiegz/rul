require 'rubygems'
require 'rake'
require 'activesupport'
require 'erb'

# TODO ARGV these suckas
APP_NAME      = "tieg".split('/').last.underscore.strip
APP_VERSION   = 0.1
APP_ID        = 'my@email.address'
APP_AUTHOR    = 'zzz'

RUL_ROOT      = File.dirname(__FILE__) + "/#{APP_NAME}"
XUL_PATH      = "/Library/Frameworks/XUL.framework"
BIN_PATH      = XUL_PATH + "/xulrunner-bin" # TODO x-platformize


TEMPLATE_ROOT = File.dirname(__FILE__), "lib", "templates"

namespace :app do
  namespace :install do
    desc "install this xulrunner application for Mac OS X"
    task :mac do
      run %(#{BIN_PATH} --install-app #{RUL_ROOT})
    end
  end

  desc "removes install files and app"
  task :reset do
    run %(rm -rf #{APP_NAME})
    # FileUtils.rm_rf "#{APP_NAME}.app"
  end

  desc "run this xulrunner application"
  task :run do
    run %(#{BIN_PATH} #{RUL_ROOT}) 
  end
  
  desc "build the skeleton for this app" # TODO replace with a bin "rul" script that generates the skeleton
  task :new do
    FileUtils.mkdir_p APP_NAME
    say "generated #{APP_NAME}/"
  
    File.open "#{APP_NAME}/application.ini", 'w' do |f|
      template = File.join TEMPLATE_ROOT, 'application.ini.erb'
      f.write ERB.new(File.open(template).read, nil, '-').result(binding)
      say "generated #{APP_NAME}/application.ini"
    end

    FileUtils.mkdir_p "#{APP_NAME}/chrome/content"
    say "generated #{APP_NAME}/chrome/"
    say "generated #{APP_NAME}/chrome/content"
    say "generated #{APP_NAME}/chrome/content"
    # FileUtils.mkdir_p "#{APP_NAME}/chrome/images"
    # say "generated #{APP_NAME}/chrome/images"
    # FileUtils.mkdir_p "#{APP_NAME}/chrome/icons/default"
    # say "generated #{APP_NAME}/chrome/icons"
    # say "generated #{APP_NAME}/chrome/icons/default"

    File.open "#{APP_NAME}/chrome/content/default.xul", 'w' do |f|
      template = File.join TEMPLATE_ROOT, 'default.xul.erb'
      f.write ERB.new(File.open(template).read, nil, '-').result(binding)
      say "generated #{APP_NAME}/chrome/content/default.xul"
    end
    File.open "#{APP_NAME}/chrome/chrome.manifest", 'w' do |f|
      template = File.join TEMPLATE_ROOT, 'chrome.manifest.erb'
      f.write ERB.new(File.open(template).read, nil, '-').result(binding)
      say "generated #{APP_NAME}/chrome/chrome.manifest"
    end
    File.open "#{APP_NAME}/chrome/content/application.js", 'w' do |f|
      template = File.join TEMPLATE_ROOT, 'application.js.erb'
      f.write ERB.new(File.open(template).read, nil, '-').result(binding)
      say "generated #{APP_NAME}/chrome/chrome.manifest"
    end
    FileUtils.mkdir_p "#{APP_NAME}/defaults/preferences"
    File.open "#{APP_NAME}/defaults/preferences/prefs.js", 'w' do |f|
      template = File.join TEMPLATE_ROOT, 'prefs.js.erb'
      f.write ERB.new(File.open(template).read, nil, '-').result(binding)
      say "generated #{APP_NAME}/defaults/preferences/prefs.js"
    end
  end
end

def say something
  puts " -> #{something}"
end

def run cmd
  say cmd
  system cmd
end