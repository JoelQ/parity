require 'bundler/gem_tasks'
require 'rspec/core/rake_task'
require File.expand_path("../lib/parity/version", __FILE__)

RSpec::Core::RakeTask.new('spec')

task default: :spec

PACKAGE_NAME = "parity"
TRAVELING_RUBY_VERSION = "20141215-2.1.5"
OSX_RUBY = "traveling-ruby-#{TRAVELING_RUBY_VERSION}-osx.tar.gz"

namespace :package do
  desc "Generate parity #{Parity::VERSION} package for OSX"
  task osx: "packaging/#{OSX_RUBY}" do
    package_dir = "#{PACKAGE_NAME}-#{Parity::VERSION}-osx"
    rm_rf package_dir

    copy_lib(package_dir)
    copy_binaries(package_dir)
    copy_shims(package_dir)
    extract_ruby(package_dir)

    sh "tar -czf #{package_dir}.tar.gz #{package_dir}"
  end
end

file "packaging/#{OSX_RUBY}" do
  sh "cd packaging && curl -L -O --fail " +
    "http://d6r77u77i8pq3.cloudfront.net/releases/#{OSX_RUBY}"
end

def copy_lib(package_dir)
  app_dir = "#{package_dir}/lib/app"
  mkdir_p app_dir

  cp_r "lib", app_dir
  cp "README.md", app_dir
end

def copy_binaries(package_dir)
  bin_path = "#{package_dir}/lib/app/bin"
  mkdir_p bin_path

  cp "bin/development", bin_path
  cp "bin/staging", bin_path
  cp "bin/production", bin_path
end

def copy_shims(package_dir)
  shim_dir = "#{package_dir}/bin"
  mkdir shim_dir
  cp "packaging/shim.sh", "#{shim_dir}/development"
end

def extract_ruby(package_dir)
  ruby_dir = "#{package_dir}/lib/ruby"
  mkdir_p ruby_dir
  sh "tar -xzf packaging/#{OSX_RUBY} -C #{ruby_dir}"
end
