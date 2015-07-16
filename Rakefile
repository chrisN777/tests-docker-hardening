# encoding: utf-8

require 'rake'
require 'rspec/core/rake_task'

# Serverspec tests
suites = Dir.glob('*').select { |entry| File.directory?(entry) }

class ServerspecTask < RSpec::Core::RakeTask
  attr_accessor :target

  def spec_command
    if target.nil?
      puts 'specify either env TARGET_HOST or target_host='
      exit 1
    end

    cmd = super
    "env TARGET_HOST=#{target} STANDALONE_SPEC=true #{cmd}  --format documentation --no-profile --color"
  end
end

namespace :serverspec do
  suites.each do |suite|
    desc "Run serverspec suite #{suite}"
    ServerspecTask.new(suite.to_sym) do |t|
      t.rspec_opts = '--no-color --format html --out report.html' if ENV['format'] == 'html'
      t.rspec_opts = '--no-color --format json --out report.json' if ENV['format'] == 'json'
      t.target = ENV['TARGET_HOST'] || ENV['target_host']
      t.ruby_opts = "-I #{suite}/serverspec"
      t.pattern = "#{suite}/serverspec/*_spec.rb"
    end
  end
end
