Capistrano 3 has a different DSL for tasks. You won't be able to incorporate DJ tasks via `require 'delayed/recipes'` in your `deploy.rb` anymore, so don't try to adding it to your new Capfile.

Instead, for a Rails project, save the following in `lib/capistrano/tasks` as `delayed_job.cap`.

```ruby
namespace :delayed_job do
 
  def args
    fetch(:delayed_job_args, "")
  end
 
  def delayed_job_roles
    fetch(:delayed_job_server_role, :app)
  end
 
  desc 'Stop the delayed_job process'
  task :stop do
    on roles(delayed_job_roles) do
      within release_path do    
        with rails_env: fetch(:rails_env) do
          execute :'script/delayed_job', :stop
        end
      end
    end
  end
 
  desc 'Start the delayed_job process'
  task :start do
    on roles(delayed_job_roles) do
      within release_path do
        with rails_env: fetch(:rails_env) do
          execute :'script/delayed_job', args, :start
        end
      end
    end
  end
 
  desc 'Restart the delayed_job process'
  task :restart do
    on roles(delayed_job_roles) do
      within release_path do
        with rails_env: fetch(:rails_env) do
          execute :'script/delayed_job', args, :restart
        end
      end
    end
  end
 
end
```

You can still use these configuration options if you wish in your deploy.rb:

```ruby
set :delayed_job_server_role, :worker
set :delayed_job_args, "-n 2"
```
