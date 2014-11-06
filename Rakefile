require 'prmd/rake_tasks/combine'
require 'prmd/rake_tasks/verify'
require 'prmd/rake_tasks/doc'

namespace :schema do
  Prmd::RakeTasks::Combine.new do |t|
    t.options[:meta] = 'meta.json'
    t.paths << 'schemata'
    t.output_file = 'schema.json'
  end

  Prmd::RakeTasks::Verify.new do |t|
    t.files << 'schema.json'
  end

  Prmd::RakeTasks::Doc.new do |t|
    t.options[:prepend] = ['preface.md']
    t.files = { 'schema.json' => 'README.md' }
  end
end

task default: ['schema:combine', 'schema:verify', 'schema:doc']
