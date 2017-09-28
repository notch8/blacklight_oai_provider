require 'rake'
require 'bundler'

Bundler::GemHelper.install_tasks

require 'rspec/core/rake_task'
require 'engine_cart/rake_task'
require 'solr_wrapper'

EngineCart.fingerprint_proc = EngineCart.rails_fingerprint_proc

task :default => :ci

desc "Run specs"
RSpec::Core::RakeTask.new

desc 'Run test suite'
task ci: ['engine_cart:generate'] do
  SolrWrapper.wrap do |solr|
    solr.with_collection(name: 'blacklight-core', dir: File.join(File.expand_path(File.dirname(__FILE__)), "solr", "conf")) do
      within_test_app do
        system "RAILS_ENV=test rake blacklight_oai_provider:index:seed"
      end
      Rake::Task['spec'].invoke
    end
  end
end
