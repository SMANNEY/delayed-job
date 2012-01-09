### Note: Create the script

If DJ is installed as a gem don't forget to run the generator. It creates the script/delayed_job script and sets run permissions.

    rails generate delayed_job

### Capistrano

delayed_job comes with Capistrano recipes to start the script/delayed_job worker but you have to update your config/deploy.rb file to use them. There are 3 changes that need to be made:

### Include the recipes  

    require "delayed/recipes"  

### The recipes use the :rails_env variable to pass the environment to script/delayed_job.   
    set :rails_env, "production" #added for delayed job  

### Only start and stop workers on a particular server

If you have one or more servers just for delayed job, you can have your workers run just on that server like so. If you do not set `:delayed_job_server_role`, it will default to `:app`.

    role :delayed_job, 'delayed_job.example.com'
    set :delayed_job_server_role, :delayed_job

### Hook into Capistrano to start, stop and restart  

    # Delayed Job  
    before "deploy:restart", "delayed_job:stop"
    after  "deploy:restart", "delayed_job:start"

    after "deploy:stop",  "delayed_job:stop"
    after "deploy:start", "delayed_job:start"

    # If you want to use command line options, for example to start multiple workers,
    # define a Capistrano variable delayed_job_args:
    #
    #   set :delayed_job_args, "-n 2"
