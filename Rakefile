require "newrelic_rpm"
require "pgbackups-archive"


class RunPgbackupsArchive
  include NewRelic::Agent::Instrumentation::ControllerInstrumentation

  def call
    Heroku::Client::PgbackupsArchive.perform
  end

  add_transaction_tracer :call, category: :task
end

namespace :pgbackups do

  desc "Perform a pgbackups backup then archive to S3."
  task :archive do
    RunPgbackupsArchive.new.call
  end

end

NewRelic::Agent.manual_start app_name: "pgbackups-archive",
  transaction_tracer: { transaction_threshold: 1.5 }
