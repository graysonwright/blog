# Add your own tasks in files placed in lib/tasks ending in .rake,
# for example lib/tasks/capistrano.rake, and they will automatically be available to Rake.

require_relative 'config/application'

Rails.application.load_tasks
task(:default).clear
task default: [:spec]

task ensure_self_destruct_is_scheduled: [:environment] do
  include ApplicationHelper

  if Post.any? && Delayed::Job.none? { |job| job.handler =~ /SelfDestruct/ }
    SelfDestruct.new.delay(run_at: expiration_time).attempt
  end
end

if defined? RSpec
  task(:spec).clear
  RSpec::Core::RakeTask.new(:spec) do |t|
    t.verbose = false
  end
end

task default: "bundler:audit"
