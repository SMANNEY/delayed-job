Start by taking a look at this gem [capistrano3-delayed-job](https://github.com/platanus/capistrano3-delayed-job), this way we won't need to repeat this code in every project that uses delayed jobs.

### If you don't want to use a gem

Capistrano 3 has a different DSL for tasks. You won't be able to incorporate DJ tasks via `require 'delayed/recipes'` in your `deploy.rb` anymore, so don't try to adding it to your new `Capfile`.

Instead, for a Rails project, save the following in `lib/capistrano/tasks` as `delayed_job.rake`.

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
          execute :bundle, :exec, :'bin/delayed_job', :stop
        end
      end
    end
  end
 
  desc 'Start the delayed_job process'
  task :start do
    on roles(delayed_job_roles) do
      within release_path do
        with rails_env: fetch(:rails_env) do
          execute :bundle, :exec, :'bin/delayed_job', args, :start
        end
      end
    end
  end
 
  desc 'Restart the delayed_job process'
  task :restart do
    on roles(delayed_job_roles) do
      within release_path do
        with rails_env: fetch(:rails_env) do
          execute :bundle, :exec, :'bin/delayed_job', args, :restart
        end
      end
    end
  end
 
end
```

**Rails 3:** *replace bin/delayed_job with script/delayed_job*

**Note:** According to the [README](https://github.com/collectiveidea/delayed_job#running-jobs), you should also ensure you have add `gem 'daemons'` to your Gemfile.

Ensure you have a `shared/tmp/pids` folder.

```ruby
set :linked_dirs, %w{tmp/pids}
```

You can still use these configuration options if you wish in your deploy.rb:

```ruby
set :delayed_job_server_role, :worker
set :delayed_job_args, "-n 2"
```

To have delayed_job restart every deploy, add the following to your deploy.rb:

```ruby
after 'deploy:publishing', 'deploy:restart'
namespace :deploy do
  task :restart do
    invoke 'delayed_job:restart'
  end
end
```
