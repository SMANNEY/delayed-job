delayed_job comes with Capistrano recipes to start the script/delayed_job worker but you have to update your config/deploy.rb file to use them. There are 3 changes that need to be made:

### Include the recipes  

    ```ruby
    require "delayed/recipes"  
    ```

### The recipes use the :rails_env variable to pass the environment to script/delayed_job.   
    ```ruby
     set :rails_env, "production" #added for delayed job  
    ```

### Hook into Capistrano to start, stop and restart  

    ```ruby 
    # Delayed Job  
    after "deploy:stop",    "delayed_job:stop"  
    after "deploy:start",   "delayed_job:start"  
    after "deploy:restart", "delayed_job:restart"  
    ```