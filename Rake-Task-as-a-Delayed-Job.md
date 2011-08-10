Here's an example how to trigger rake tasks with delayed_job. Create the following file and require it from the _application.rb_ or _environment.rb_
    
    # lib/delayed_rake.rb
    class DelayedRake < Struct.new(:task, :options)
      def perform
        env_options = ''
        options && options.stringify_keys!.each do |key, value|
          env_options << " #{key.upcase}=#{value}"
        end
        system("cd #{Rails.root} && RAILS_ENV=#{Rails.env} bundle exec rake #{task} #{env_options} >> log/delayed_rake.log")
      end
    end

Then you can put it on the queue like so:

    Delayed::Job.enqueue(DelayedRake.new("paperclip:refresh:metadata", :class => 'Avatar'))

